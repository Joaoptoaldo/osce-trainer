# Arquitetura do sistema

**Projeto:** OSCE Trainer  
**Versão documental:** 1.3  
**Estilo:** monólito modular Django com arquitetura em camadas  
**Stack definida:** Python, Django, Django Templates, Bootstrap 5, PostgreSQL e provedor de IA (LLM)

## 1. Objetivo arquitetural

Construir uma aplicação web de treinamento clínico individual, multiusuário e extensível. O produto não é um chatbot médico nem um sistema de gestão acadêmica.

Estudantes podem criar estações de diferentes especialidades e origens, executar tentativas independentes e receber avaliações vinculadas à rubrica utilizada. Síndrome Coronariana é o primeiro caso-piloto, não uma especialização do código. Tuberculose Pediátrica será o segundo teste de generalização.

## 2. Princípios

1. Separar responsabilidades em camadas.
2. Organizar o domínio por módulos de negócio, sem transformar cada responsabilidade técnica em um app Django.
3. Manter regras clínicas e de autorização no backend.
4. Tratar a IA como colaborador probabilístico, nunca como fonte de verdade clínica.
5. Preservar versões imutáveis e histórico de tentativas.
6. Usar persistência relacional para entidades e relacionamentos; JSONB para estruturas clínicas variáveis e validadas.
7. Aplicar isolamento de dados por proprietário.
8. Preferir soluções explícitas, testáveis e simples.
9. Não antecipar microserviços, mensageria distribuída, plugins ou abstrações sem necessidade comprovada.

## 3. Arquitetura em camadas

### 3.1. Domínio (`domain/`)

Responsabilidade: representar conceitos, invariantes e regras centrais do produto.

Módulos de domínio:
- `accounts`: identidade e conceitos de usuário/perfil.
- `stations`: estação, versão, procedência, caso clínico e critérios.
- `simulations`: tentativa, mensagens, ações e recursos solicitados.
- `evaluations`: avaliação e resultado por critério.

Os módulos de domínio não dependem da apresentação nem chamam diretamente o SDK do provedor de IA.

**Decisão pragmática Django:** modelos ORM podem residir nos módulos de domínio para aproveitar o Django sem duplicar modelos em uma camada de persistência separada. Isso cria acoplamento consciente ao ORM. Regras importantes não devem ficar espalhadas em templates e views.

### 3.2. Aplicação (`application/`)

Responsabilidade: coordenar casos de uso e transações.

Exemplos:
- criar/editar estação;
- validar completude;
- criar nova versão;
- duplicar estação;
- iniciar tentativa;
- processar mensagem;
- solicitar recurso clínico;
- encerrar tentativa;
- gerar avaliação;
- retomar tentativa aberta.

Pode conter serviços de aplicação, comandos, consultas e DTOs quando úteis. Não criar classe para cada CRUD simples. Não conter código HTTP, templates ou chamadas diretas espalhadas ao SDK do provedor de IA.

### 3.3. Infraestrutura (`infrastructure/`)

Responsabilidade: implementar integrações técnicas externas e detalhes operacionais.

No MVP:
- adaptador do provedor de IA;
- cliente e configuração do provedor;
- validação/serialização dos contratos externos;
- logging técnico e métricas de integração.

A integração fica isolada atrás de um contrato interno. Não haverá suporte multi-provider configurável no MVP.

### 3.4. Apresentação (`presentation/`)

Responsabilidade: interface e protocolo de interação com o usuário.

No MVP:
- Django Views;
- Forms;
- URLs;
- templates Django;
- Bootstrap 5;
- apresentação de validações e estados.

Views recebem requisições, validam entrada, invocam casos de uso e escolhem a resposta HTTP. Não devem conter regras clínicas, orquestração extensa ou chamadas diretas ao provedor de IA.

### 3.5. Configuração e composição (`config/`)

Contém settings, URLs raiz, ASGI/WSGI, registro de apps, composição e configurações por ambiente.

### Observações

- `domain/*` contém módulos registrados como Django apps quando precisarem de models, migrations e integração com o app registry.
- `application/*` agrupa casos de uso por módulo de negócio.
- `infrastructure/*` contém implementações de integrações externas.
- `presentation/*` concentra interface e protocolo HTTP.
- `config/*` compõe a aplicação Django.
- Não criar diretórios vazios antecipadamente. `selectors`, `services`, `ports`, `repositories` e DTOs só surgem quando houver necessidade.
- A pasta `apps/` não será usada como recipiente obrigatório de todas as responsabilidades.
- O uso de ORM no domínio é pragmático, não uma alegação de domínio puro independente do Django.

## 4. Direção das dependências

```text
presentation ───────> application ───────> domain
                         │
                         ▼
                   infrastructure
```

A aplicação define os contratos necessários para operações externas. A infraestrutura implementa esses contratos. A apresentação não acessa diretamente o SDK do provedor de IA.

Restrições:
- domínio não importa apresentação nem SDKs externos;
- aplicação não importa Views, Forms ou templates;
- apresentação não implementa regras de domínio;
- infraestrutura não decide regras clínicas;
- evitar dependências circulares entre módulos.

## 5. Modelo conceitual

| Entidade | Responsabilidade |
|---|---|
| `User` | Identidade, autenticação e perfil |
| `Station` | Identidade permanente e proprietário |
| `StationVersion` | Fotografia imutável da configuração |
| `ClinicalCase` | Dados clínicos estruturados da versão |
| `ChecklistCriterion` | Critério avaliativo da versão |
| `Attempt` | Execução independente de uma versão |
| `ConversationMessage` | Mensagem integral da tentativa |
| `RequestedResource` | Solicitação e liberação de exame/recurso |
| `Assessment` | Avaliação da tentativa |
| `CriterionAssessment` | Resultado e evidências por critério |

Relacionamentos:

```text
User 1 ── N Station
Station 1 ── N StationVersion
StationVersion 1 ── 1 ClinicalCase
StationVersion 1 ── N ChecklistCriterion
StationVersion 1 ── N Attempt
Attempt 1 ── N ConversationMessage
Attempt 1 ── N RequestedResource
Attempt 1 ── 0..1 Assessment
Assessment 1 ── N CriterionAssessment
```

Nomes e cardinalidades finais serão confirmados na modelagem física. Não criar entidades apenas para refletir cada substantivo documental.

## 6. Autoria, origem e procedência

Autoria e origem são conceitos diferentes.

- `Station.owner`: proprietário.
- `StationVersion.source_type`: origem do conteúdo daquela versão.
- `source_reference`: referência original, quando houver.
- `structured_by`: responsável pela estruturação.
- `created_at`: data de criação da versão.
- `clinical_validation_status`: situação de validação.
- `validation_notes`: observações e pendências.

Valores iniciais de `source_type`:
- `student_created`;
- `academic_material`;
- `adapted`;
- `imported`.

A procedência pertence à versão, pois uma versão futura pode ter origem ou adaptação diferente. Origem não significa validação clínica.

## 7. Versionamento e fotografia da tentativa

`Station` é identidade permanente. `StationVersion` é configuração imutável. `Attempt` referencia exatamente uma versão.

A versão preserva apresentação inicial, ficha clínica, recursos e resultados, regras de divulgação, comportamento do paciente, checklist e metadados de origem/validação.

Depois de utilizada, uma versão não pode ser sobrescrita. Alterações relevantes criam nova versão.

A tentativa preserva versão, transcrição integral, recursos solicitados/liberados, estado, timestamps e avaliação relacionada. Edições posteriores não alteram o histórico.

Não introduzir event sourcing no MVP.

## 8. Persistência relacional e JSONB

Relacional:
- usuários, estações, versões, critérios, tentativas, mensagens, recursos, avaliações e resultados.

JSONB:
- história da doença atual;
- antecedentes;
- sinais vitais;
- exame físico;
- resultados de exames;
- regras variáveis de divulgação;
- blocos clínicos específicos.

Cada estrutura JSON deve ter schema/versionamento de formato e validação no backend. Critérios, tentativas e mensagens continuam como entidades próprias.

## 9. Simulação e responsabilidades da IA

Componentes lógicos, não necessariamente classes ou apps separados:

| Componente | Responsabilidade |
|---|---|
| `IntentInterpreter` | Interpretar mensagem em intenção estruturada |
| `ClinicalStateManager` | Verificar validade da ação e disponibilidade do dado |
| `PatientSimulator` | Formular resposta natural com dados autorizados |
| `ResponseParser` | Validar formato e campos retornados |
| `StationEvaluator` | Avaliar tentativa encerrada segundo rubrica |

Fluxo:
1. Validar usuário, propriedade e estado da tentativa.
2. Persistir mensagem com estratégia de idempotência.
3. Interpretar intenção por contrato estruturado.
4. Validar intenção e suficiência da solicitação no backend.
5. Consultar somente dados necessários e autorizados.
6. Liberar ou negar recurso conforme configuração.
7. Formular resposta natural limitada aos dados liberados.
8. Validar e persistir resposta e ação.
9. Retornar resposta.

A ficha integral não deve ser enviada à IA em toda interação. Conteúdo cadastrado pelo usuário é dado não confiável, nunca instrução de sistema.

## 10. Avaliação

Somente após encerramento explícito:
1. bloquear novas mensagens de atendimento;
2. recuperar transcrição integral e versão exata;
3. recuperar critérios daquela versão;
4. solicitar avaliação estruturada;
5. validar evidências contra mensagens reais;
6. persistir avaliação e resultados;
7. apresentar relatório.

Classificações:
- `completed`: Realizado;
- `partial`: Parcialmente realizado;
- `not_completed`: Não realizado;
- `not_evaluable`: Não foi possível avaliar.

Não inferir execução sem evidência. Não inventar pontuação sem fórmula oficial.

## 11. Resiliência, concorrência e idempotência

- Persistir mensagem antes da chamada externa.
- Operações repetíveis devem possuir estratégia de idempotência.
- Evitar duplicação de resposta, ação ou liberação de exame.
- Falha do provedor de IA mantém tentativa recuperável.
- Não marcar interação concluída antes de validar e persistir a resposta.
- Tentativa interrompida pode ser retomada enquanto aberta e autorizada.
- Encerramento e avaliação devem impedir execução concorrente duplicada.

## 12. Interface e testes

Interface: Django Templates e Bootstrap 5. Telas: autenticação, dashboard, minhas estações, edição, simulação, avaliação, histórico e administração.

Testar isolamento, permissões, procedência, imutabilidade, duplicação, schemas, solicitações ambíguas, liberação de recursos, falhas do provedor de IA, retomada, idempotência, concorrência, encerramento, evidências e generalização dos dois casos-piloto.

## 13. Pendências antes da implementação

- Verificar versões Python/Django e dependências.
- Definir schemas JSON iniciais.
- Definir estados de estação, versão e tentativa.
- Definir limites do provedor de IA e política de retry.
- Definir regra de publicação/congelamento.
- Validar clinicamente materiais antes de marcá-los como validados.
