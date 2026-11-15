# Arquitetura do sistema

**Projeto:** OSCE Trainer  
**Versão documental:** 1.6  
**Data:** 2026-11-15  
**Estilo:** monólito modular Django com arquitetura em camadas  
**Stack definida:** Python, Django, Django Templates, Bootstrap 5, PostgreSQL e provedor de IA (LLM — **pendente: escolha do provedor/biblioteca**)

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
- `estacoes`: estação, versão, procedência, caso clínico e critérios.
- `simulacoes`: tentativa, mensagens, ações e recursos solicitados.
- `avaliacoes`: avaliação e resultado por critério.

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

| Entidade | Classe | Tabela | Responsabilidade |
|---|---|---|---|
| `Usuario` | `Usuario` | `usuarios` | Identidade, autenticação e perfil |
| `Estacao` | `Estacao` | `estacoes` | Identidade permanente e proprietário |
| `VersaoEstacao` | `VersaoEstacao` | `versoes_estacao` | Fotografia imutável da configuração |
| `CasoClinico` | `CasoClinico` | `casos_clinicos` | Dados clínicos estruturados da versão |
| `CriterioAvaliacao` | `CriterioAvaliacao` | `criterios_avaliacao` | Critério avaliativo da versão |
| `Tentativa` | `Tentativa` | `tentativas` | Execução independente de uma versão |
| `MensagemConversa` | `MensagemConversa` | `mensagens_conversa` | Mensagem integral da tentativa |
| `RecursoSolicitado` | `RecursoSolicitado` | `recursos_solicitados` | Solicitação e liberação de exame/recurso |
| `Avaliacao` | `Avaliacao` | `avaliacoes` | Avaliação da tentativa |
| `ResultadoCriterio` | `ResultadoCriterio` | `resultados_criterios` | Resultado e evidências por critério |

Relacionamentos:

```text
Usuario 1 ── N Estacao
Estacao 1 ── N VersaoEstacao
VersaoEstacao 1 ── 1 CasoClinico
VersaoEstacao 1 ── N CriterioAvaliacao
VersaoEstacao 1 ── N Tentativa
Tentativa 1 ── N MensagemConversa
Tentativa 1 ── N RecursoSolicitado
Tentativa 1 ── 0..1 Avaliacao
Avaliacao 1 ── N ResultadoCriterio
```

Nomes e cardinalidades finais serão confirmados na modelagem física. Não criar entidades apenas para refletir cada substantivo documental.

### 5.1 Modelagem física proposta (pendente de aprovação)

> **Nota:** A modelagem física abaixo é uma **proposta** sujeita a validação. Não criar migrações antes de confirmar com a equipe.
>
> **Convenção:** Nomes de tabelas e colunas em `snake_case` português. Identificadores sem acentos (ex: `avaliacao`, `nao_avaliavel`, `criado_em`). Valores de enumeração em `snake_case` português. Classes Django em `PascalCase` português (ex: `Usuario`, `VersaoEstacao`).
>
> **Nota sobre `Usuario`:** Django gerencia a tabela de usuários via migrations (`AbstractUser` ou modelo customizado). A especificação abaixo é conceitual; `senha_hash` representa o campo `password` do Django. O mapeamento real deve seguir as convenções do Django (`password`, `is_staff`, `is_active`, `is_superuser`, `date_joined`, `last_login`).

#### Tabelas e colunas principais

```sql
-- usuarios (compatível com Django AbstractUser customizado)
-- NOTA: Django gerencia a tabela de usuários via migrations; esta é a especificação conceitual
CREATE TABLE usuarios (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    email VARCHAR(255) UNIQUE NOT NULL,
    username VARCHAR(150) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL,
    is_staff BOOLEAN DEFAULT FALSE,
    is_active BOOLEAN DEFAULT TRUE,
    is_superuser BOOLEAN DEFAULT FALSE,
    date_joined TIMESTAMPTZ DEFAULT NOW(),
    last_login TIMESTAMPTZ,
    criado_em TIMESTAMPTZ DEFAULT NOW(),
    atualizado_em TIMESTAMPTZ DEFAULT NOW()
);

-- estacoes (identidade permanente)
CREATE TABLE estacoes (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    proprietario_id UUID NOT NULL REFERENCES usuarios(id) ON DELETE CASCADE,
    titulo VARCHAR(255) NOT NULL,
    especialidade VARCHAR(100),
    publico_alvo VARCHAR(100),
    status VARCHAR(20) NOT NULL DEFAULT 'rascunho',
    CONSTRAINT chk_estacoes_status CHECK (status IN ('rascunho', 'publicada', 'arquivada')),
    criado_em TIMESTAMPTZ DEFAULT NOW(),
    atualizado_em TIMESTAMPTZ DEFAULT NOW(),
    arquivado_em TIMESTAMPTZ
);

-- versoes_estacao (configuração imutável)
CREATE TABLE versoes_estacao (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    estacao_id UUID NOT NULL REFERENCES estacoes(id) ON DELETE CASCADE,
    numero_versao INTEGER NOT NULL,
    status VARCHAR(20) NOT NULL DEFAULT 'rascunho',
    CONSTRAINT chk_versoes_estacao_status CHECK (status IN ('rascunho', 'publicada', 'substituida')),
    tipo_origem VARCHAR(30) NOT NULL,
    CONSTRAINT chk_versoes_estacao_tipo_origem CHECK (tipo_origem IN ('criada_pelo_estudante', 'material_academico', 'adaptada', 'importada')),
    referencia_origem TEXT,
    estruturado_por_id UUID REFERENCES usuarios(id),
    estado_validacao_clinica VARCHAR(20) NOT NULL DEFAULT 'pendente',
    CONSTRAINT chk_versoes_estacao_validacao_clinica CHECK (estado_validacao_clinica IN ('pendente', 'validada', 'rejeitada', 'precisa_revisao')),
    validado_por_id UUID REFERENCES usuarios(id),
    validado_em TIMESTAMPTZ,
    observacoes_validacao TEXT,
    criado_em TIMESTAMPTZ DEFAULT NOW(),
    publicado_em TIMESTAMPTZ,
    substituido_em TIMESTAMPTZ,
    UNIQUE (estacao_id, numero_versao)
);

-- casos_clinicos (dados clínicos estruturados - JSONB com schema)
CREATE TABLE casos_clinicos (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    versao_estacao_id UUID NOT NULL UNIQUE REFERENCES versoes_estacao(id) ON DELETE CASCADE,
    apresentacao TEXT NOT NULL,
    instrucoes TEXT,
    perfil_paciente JSONB NOT NULL,
    historico JSONB NOT NULL,
    sinais_vitais JSONB,
    exame_fisico JSONB,
    regras_divulgacao JSONB NOT NULL DEFAULT '{}',
    recursos JSONB NOT NULL DEFAULT '[]',
    criado_em TIMESTAMPTZ DEFAULT NOW()
);

-- criterios_avaliacao
CREATE TABLE criterios_avaliacao (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    versao_estacao_id UUID NOT NULL REFERENCES versoes_estacao(id) ON DELETE CASCADE,
    codigo VARCHAR(20) NOT NULL,
    descricao TEXT NOT NULL,
    ordem_indice INTEGER NOT NULL DEFAULT 0,
    avaliavel BOOLEAN NOT NULL DEFAULT TRUE,
    razao_nao_avaliavel TEXT,
    criado_em TIMESTAMPTZ DEFAULT NOW(),
    UNIQUE (versao_estacao_id, codigo)
);

-- tentativas
CREATE TABLE tentativas (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    versao_estacao_id UUID NOT NULL REFERENCES versoes_estacao(id) ON DELETE RESTRICT,
    estudante_id UUID NOT NULL REFERENCES usuarios(id) ON DELETE CASCADE,
    status VARCHAR(20) NOT NULL DEFAULT 'em_andamento',
    CONSTRAINT chk_tentativas_status CHECK (status IN ('em_andamento', 'concluida', 'abandonada', 'arquivada')),
    iniciado_em TIMESTAMPTZ DEFAULT NOW(),
    concluido_em TIMESTAMPTZ,
    abandonado_em TIMESTAMPTZ,
    arquivado_em TIMESTAMPTZ,
    janela_recuperacao_expira_em TIMESTAMPTZ,
    criado_em TIMESTAMPTZ DEFAULT NOW()
);

-- mensagens_conversa
CREATE TABLE mensagens_conversa (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    tentativa_id UUID NOT NULL REFERENCES tentativas(id) ON DELETE CASCADE,
    sequencia INTEGER NOT NULL,
    papel VARCHAR(20) NOT NULL,
    CONSTRAINT chk_mensagens_conversa_papel CHECK (papel IN ('estudante', 'paciente', 'sistema')),
    conteudo TEXT NOT NULL,
    dados_intencao JSONB,
    dados_acao JSONB,
    criado_em TIMESTAMPTZ DEFAULT NOW(),
    UNIQUE (tentativa_id, sequencia)
);

-- recursos_solicitados
CREATE TABLE recursos_solicitados (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    tentativa_id UUID NOT NULL REFERENCES tentativas(id) ON DELETE CASCADE,
    mensagem_id UUID NOT NULL REFERENCES mensagens_conversa(id),
    tipo_recurso VARCHAR(50) NOT NULL,
    CONSTRAINT chk_recursos_solicitados_tipo CHECK (tipo_recurso IN ('laboratorio', 'imagem', 'exame_fisico', 'ecg', 'outro')),
    chave_recurso VARCHAR(100) NOT NULL,
    status VARCHAR(20) NOT NULL DEFAULT 'solicitado',
    CONSTRAINT chk_recursos_solicitados_status CHECK (status IN ('solicitado', 'liberado', 'negado', 'pendente_esclarecimento')),
    dados_liberados JSONB,
    razao_negativa TEXT,
    solicitado_em TIMESTAMPTZ DEFAULT NOW(),
    liberado_em TIMESTAMPTZ,
    chave_idempotencia VARCHAR(100) UNIQUE
);

-- avaliacoes
CREATE TABLE avaliacoes (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    tentativa_id UUID NOT NULL UNIQUE REFERENCES tentativas(id) ON DELETE CASCADE,
    status VARCHAR(20) NOT NULL DEFAULT 'pendente',
    CONSTRAINT chk_avaliacoes_status CHECK (status IN ('pendente', 'concluida', 'falhou')),
    tipo_avaliador VARCHAR(20) NOT NULL,
    CONSTRAINT chk_avaliacoes_tipo_avaliador CHECK (tipo_avaliador IN ('ia', 'humano')),
    versao_avaliador VARCHAR(50),
    resumo TEXT,
    iniciado_em TIMESTAMPTZ DEFAULT NOW(),
    concluido_em TIMESTAMPTZ,
    falhou_em TIMESTAMPTZ,
    mensagem_erro TEXT
);

-- resultados_criterios
CREATE TABLE resultados_criterios (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    avaliacao_id UUID NOT NULL REFERENCES avaliacoes(id) ON DELETE CASCADE,
    criterio_id UUID NOT NULL REFERENCES criterios_avaliacao(id),
    classificacao VARCHAR(20) NOT NULL,
    CONSTRAINT chk_resultados_criterios_classificacao CHECK (classificacao IN ('realizado', 'parcial', 'nao_realizado', 'nao_avaliavel')),
    referencias_evidencias UUID[] NOT NULL DEFAULT '{}',
    justificativa TEXT,
    criado_em TIMESTAMPTZ DEFAULT NOW(),
    UNIQUE (avaliacao_id, criterio_id)
);
```

#### Restrições adicionais de integridade (propostas)

```sql
-- Apenas uma versão publicada atual por estação
CREATE UNIQUE INDEX idx_versoes_estacao_publicada_atual
    ON versoes_estacao (estacao_id)
    WHERE status = 'publicada';

-- Mensagens: sequencia NOT NULL já garantido por UNIQUE + NOT NULL implícito
-- A constraint UNIQUE (tentativa_id, sequencia) garante unicidade e ordem por tentativa

-- Evidências devem referenciar mensagens existentes (validado na aplicação)
-- FK direta não usada pois referencias_evidencias é array; validação no backend (RB-042)

-- Regra de exclusão e preservação do histórico:
-- - VersaoEstacao: ON DELETE RESTRICT se houver Tentativas vinculadas (já definido)
-- - Tentativa: ON DELETE CASCADE para mensagens, recursos, avaliação (histórico preservado via CASCADE)
-- - Estacao: ON DELETE CASCADE para versões (mas versões com tentativas bloqueiam via RESTRICT)
```

#### Índices recomendados

```sql
-- Isolamento por proprietário (queries frequentes)
CREATE INDEX idx_estacoes_proprietario ON estacoes(proprietario_id);
CREATE INDEX idx_tentativas_estudante ON tentativas(estudante_id);
CREATE INDEX idx_tentativas_versao_estacao ON tentativas(versao_estacao_id);

-- Versionamento
CREATE INDEX idx_versoes_estacao_estacao ON versoes_estacao(estacao_id, numero_versao DESC);
CREATE INDEX idx_versoes_estacao_status ON versoes_estacao(status) WHERE status = 'publicada';

-- Tentativas e mensagens
CREATE INDEX idx_mensagens_conversa_tentativa ON mensagens_conversa(tentativa_id, sequencia);
CREATE INDEX idx_recursos_solicitados_tentativa ON recursos_solicitados(tentativa_id);

-- Auditoria e recuperação
CREATE INDEX idx_tentativas_status_recuperacao ON tentativas(status, janela_recuperacao_expira_em) 
    WHERE status IN ('abandonada', 'em_andamento');
```

#### Schemas JSONB (propostos — requerem validação)

> **Convenção:** Propriedades JSON em `snake_case` português, sem acentos. Cada schema indica campos obrigatórios (req) e opcionais (opt) na descrição.

**PerfilPaciente** (req)
```json
{
  "nome": "string",
  "idade": "integer",
  "sexo": "M|F|outro",
  "persona": "string",
  "queixa_principal": "string"
}
```

**HistoricoClinico** (req)
```json
{
  "doenca_atual": "string",
  "inicio_minutos_atras": "integer",
  "localizacao": "string",
  "irradiacao": "string",
  "qualidade": "string",
  "intensidade": "integer",
  "sintomas_associados": ["string"],
  "antecedentes_pessoais": {"tem_has": "boolean", "medicamentos": ["string"]},
  "habitos": {"tabagismo": "string", "alcool": "string"},
  "historico_familiar": "string",
  "alergias": ["string"]
}
```

**SinaisVitais** (opt global; req se cenário exige)
```json
{
  "pas": "integer",
  "pad": "integer",
  "fc": "integer",
  "fr": "integer",
  "spo2": "integer",
  "temperatura": "number"
}
```

**ExameFisico** (opt global)
```json
{
  "geral": "string",
  "cardiaco": "string",
  "pulmonar": "string",
  "abdominal": "string",
  "extremidades": "string"
}
```

**RegrasDivulgacao** (req)
```json
{
  "sinais_vitais": {"liberacao_automatica": true},
  "exame_fisico": {"requer_solicitacao_explicita": true},
  "resultados_laboratorio": {"requer_solicitacao_explicita": true, "atraso_minutos": 0},
  "ecg": {"requer_solicitacao_explicita": true, "nunca_liberar_achados": true}
}
```

**Recurso** (item do array recursos) (opt global)
```json
{
  "tipo": "laboratorio|imagem|exame_fisico|ecg|outro",
  "chave": "string",
  "rotulo": "string",
  "dados": "object",
  "disponivel": "boolean"
}
```

**Pendências de modelagem:**
- [ ] Confirmar uso de UUID vs BIGSERIAL para PKs
- [ ] Definir `janela_recuperacao_expira_em` cálculo (ex: 24h após `abandonado_em`)
- [ ] Validar schemas JSONB com equipe clínica e técnica
- [ ] Confirmar `estado_validacao_clinica` valores e transições
- [ ] Definir se `RecursoSolicitado.chave_idempotencia` é gerado pelo backend ou cliente
- [ ] Avaliar necessidade de tabela `versao_estacao_recursos` separada vs JSONB

## 6. Autoria, origem e procedência

Autoria e origem são conceitos diferentes.

- `Estacao.proprietario_id`: proprietário.
- `VersaoEstacao.tipo_origem`: origem do conteúdo daquela versão.
- `referencia_origem`: referência original, quando houver.
- `estruturado_por_id`: responsável pela estruturação.
- `criado_em`: data de criação da versão.
- `estado_validacao_clinica`: situação de validação.
- `observacoes_validacao`: observações e pendências.

Valores iniciais de `tipo_origem`:
- `criada_pelo_estudante`;
- `material_academico`;
- `adaptada`;
- `importada`.

A procedência pertence à versão, pois uma versão futura pode ter origem ou adaptação diferente. Origem não significa validação clínica.

## 7. Versionamento e fotografia da tentativa

`Estacao` é identidade permanente. `VersaoEstacao` é configuração imutável. `Tentativa` referencia exatamente uma versão.

A versão preserva apresentação inicial, ficha clínica, recursos e resultados, regras de divulgação, comportamento do paciente, checklist e metadados de origem/validação.

Depois de utilizada, uma versão não pode ser sobrescrita. Alterações relevantes criam nova versão.

A tentativa preserva versão, transcrição integral, recursos solicitados/liberados, estado, timestamps e avaliação relacionada. Edições posteriores não alteram o histórico.

Não introduzir event sourcing no MVP.

## 8. Persistência relacional e JSONB

Relacional:
- usuarios, estacoes, versoes_estacao, criterios_avaliacao, tentativas, mensagens_conversa, recursos_solicitados, avaliacoes, resultados_criterios.

JSONB:
- historia da doença atual (historico.doenca_atual);
- antecedentes (historico.antecedentes_pessoais, historico.habitos, historico.historico_familiar);
- sinais vitais (sinais_vitais);
- exame físico (exame_fisico);
- resultados de exames (recursos.dados);
- regras variáveis de divulgação (regras_divulgacao);
- blocos clínicos específicos (perfil_paciente, historico).

Cada estrutura JSON deve ter schema/versionamento de formato e validação no backend. Critérios, tentativas e mensagens continuam como entidades próprias.

## 9. Simulação e responsabilidades da IA

Componentes lógicos, não necessariamente classes ou apps separados:

| Componente | Responsabilidade |
|---|---|
| `InterpretadorIntencao` | Interpretar mensagem em intenção estruturada |
| `GerenciadorEstadoClinico` | Verificar validade da ação e disponibilidade do dado |
| `SimuladorPaciente` | Formular resposta natural com dados autorizados |
| `ValidadorResposta` | Validar formato e campos retornados |
| `AvaliadorEstacao` | Avaliar tentativa encerrada segundo rubrica |

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

### 9.1 Contratos da integração com IA (schemas propostos — JSON Schema)

> **Proposta sujeita a aprovação e definição de provedor.** Não escolher provedor/biblioteca por conta própria.
> 
> Os schemas abaixo usam JSON Schema (draft 2020-12) para definir estrutura, tipos, campos obrigatórios, enumerações e restrições. Exemplos de valores são ilustrativos apenas.
>
> **Convenção:** Propriedades JSON em `snake_case` português, sem acentos. Valores de enumeração em `snake_case` português. Referências internas via `$defs`.

#### Componentes que utilizam LLM (probabilísticos)
- `InterpretadorIntencao` — interpreta linguagem natural → intenção estruturada
- `SimuladorPaciente` — formula resposta natural do paciente com dados autorizados
- `AvaliadorEstacao` — avalia tentativa encerrada segundo rubrica

#### Serviços determinísticos do backend (não usam LLM)
- `GerenciadorEstadoClinico` — autorização determinística de acesso e liberação de dados
- `ValidadorResposta` — validação determinística de formato e campos

> **Regra:** A IA **não** decide permissões, altera estados ou autoriza recursos. `GerenciadorEstadoClinico` é a autoridade para liberação. `ValidadorResposta` valida antes de persistir. Encerramento da tentativa depende de ação explícita da estudante, validada pelo backend.

---

#### InterpretadorIntencao (LLM)

**Input Schema**
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "required": ["mensagem", "contexto"],
  "properties": {
    "mensagem": { "type": "string", "minLength": 1 },
    "contexto": {
      "type": "object",
      "required": ["id_tentativa", "id_versao_estacao", "sequencia_mensagem"],
      "properties": {
        "id_tentativa": { "type": "string", "format": "uuid" },
        "id_versao_estacao": { "type": "string", "format": "uuid" },
        "sequencia_mensagem": { "type": "integer", "minimum": 1 },
        "mensagens_recentes": {
          "type": "array",
          "items": {
            "type": "object",
            "required": ["papel", "conteudo"],
            "properties": {
              "papel": { "type": "string", "enum": ["estudante", "paciente"] },
              "conteudo": { "type": "string" }
            }
          }
        },
        "resumo_estacao": { "type": "string" }
      }
    }
  }
}
```

**Output Schema**
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "required": ["intencao", "confianca"],
  "properties": {
    "intencao": { "type": "string", "enum": ["perguntar_anamnese", "solicitar_exame", "solicitar_exame_fisico", "emitir_hipotese", "solicitar_ecg", "solicitar_laboratorio", "encerrar_estacao", "outro"] },
    "recurso_alvo": { "type": ["string", "null"] },
    "parametros": { "type": "object" },
    "confianca": { "type": "number", "minimum": 0, "maximum": 1 }
  }
}
```

---

#### GerenciadorEstadoClinico (determinístico — backend)

**Input Schema**
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "required": ["intencao", "instantaneo_versao_estacao", "estado_tentativa"],
  "properties": {
    "intencao": { "$ref": "#/$defs/IntentOutput" },
    "instantaneo_versao_estacao": {
      "type": "object",
      "required": ["id_caso_clinico", "regras_divulgacao", "recursos"],
      "properties": {
        "id_caso_clinico": { "type": "string", "format": "uuid" },
        "regras_divulgacao": { "type": "object" },
        "recursos": { "type": "array" }
      }
    },
    "estado_tentativa": {
      "type": "object",
      "required": ["id", "status", "recursos_liberados", "sequencia_mensagem"],
      "properties": {
        "id": { "type": "string", "format": "uuid" },
        "status": { "type": "string", "enum": ["em_andamento"] },
        "recursos_liberados": { "type": "array", "items": { "type": "string" } },
        "sequencia_mensagem": { "type": "integer" }
      }
    }
  },
  "$defs": {
    "IntentOutput": {
      "type": "object",
      "required": ["intencao", "confianca"],
      "properties": {
        "intencao": { "type": "string" },
        "recurso_alvo": { "type": ["string", "null"] },
        "parametros": { "type": "object" },
        "confianca": { "type": "number" }
      }
    }
  }
}
```

**Output Schema**
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "required": ["autorizado"],
  "oneOf": [
    {
      "properties": {
        "autorizado": { "const": true },
        "dados_liberados": { "type": "object" },
        "razao_negativa": { "type": "null" }
      }
    },
    {
      "properties": {
        "autorizado": { "const": false },
        "dados_liberados": { "type": "null" },
        "razao_negativa": { "type": "string", "enum": ["recurso_nao_disponivel", "solicitacao_ambigua", "nao_investigado", "ja_liberado"] }
      }
    }
  ]
}
```

---

#### SimuladorPaciente (LLM)

**Input Schema**
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "required": ["intencao", "dados_autorizados", "perfil_paciente", "historico_conversa"],
  "properties": {
    "intencao": { "$ref": "#/$defs/IntentOutput" },
    "dados_autorizados": { "type": "object" },
    "perfil_paciente": {
      "type": "object",
      "required": ["nome", "idade", "persona", "queixa_principal"],
      "properties": {
        "nome": { "type": "string" },
        "idade": { "type": "integer", "minimum": 0 },
        "persona": { "type": "string" },
        "queixa_principal": { "type": "string" }
      }
    },
    "historico_conversa": {
      "type": "array",
      "items": {
        "type": "object",
        "required": ["papel", "conteudo"],
        "properties": {
          "papel": { "type": "string", "enum": ["estudante", "paciente"] },
          "conteudo": { "type": "string" }
        }
      }
    }
  },
  "$defs": {
    "IntentOutput": {
      "type": "object",
      "required": ["intencao", "confianca"],
      "properties": {
        "intencao": { "type": "string" },
        "recurso_alvo": { "type": ["string", "null"] },
        "parametros": { "type": "object" },
        "confianca": { "type": "number" }
      }
    }
  }
}
```

**Output Schema**
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "required": ["texto_resposta", "chaves_dados_revelados"],
  "properties": {
    "texto_resposta": { "type": "string", "minLength": 1 },
    "estado_emocional": { "type": "string", "enum": ["ansioso", "calmo", "confuso", "dor_intensa", "neutro"] },
    "chaves_dados_revelados": { "type": "array", "items": { "type": "string" } }
  }
}
```

---

#### ValidadorResposta (determinístico — backend)

**Input Schema**
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "required": ["resposta_bruta", "schema_esperado"],
  "properties": {
    "resposta_bruta": { "type": "string" },
    "schema_esperado": { "type": "string", "enum": ["RespostaPaciente", "ResultadoAvaliacao"] }
  }
}
```

**Output Schema**
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "required": ["valido"],
  "oneOf": [
    {
      "properties": {
        "processado": { "type": "object" },
        "valido": { "const": true },
        "erros": { "type": "null" }
      }
    },
    {
      "properties": {
        "processado": { "type": "null" },
        "valido": { "const": false },
        "erros": { "type": "array", "items": { "type": "string" }, "minItems": 1 }
      }
    }
  ]
}
```

---

#### AvaliadorEstacao (LLM)

**Input Schema**
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "required": ["transcricao", "rubrica", "instantaneo_versao_estacao"],
  "properties": {
    "transcricao": {
      "type": "array",
      "items": {
        "type": "object",
        "required": ["papel", "conteudo", "id_mensagem"],
        "properties": {
          "papel": { "type": "string", "enum": ["estudante", "paciente"] },
          "conteudo": { "type": "string" },
          "id_mensagem": { "type": "string", "format": "uuid" }
        }
      }
    },
    "rubrica": {
      "type": "array",
      "items": {
        "type": "object",
        "required": ["id_criterio", "codigo", "descricao", "avaliavel"],
        "properties": {
          "id_criterio": { "type": "string", "format": "uuid" },
          "codigo": { "type": "string" },
          "descricao": { "type": "string" },
          "avaliavel": { "type": "boolean" }
        }
      }
    },
    "instantaneo_versao_estacao": { "type": "object" }
  }
}
```

**Output Schema**
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "required": ["resultados_criterios", "resumo"],
  "properties": {
    "resultados_criterios": {
      "type": "array",
      "items": {
        "type": "object",
        "required": ["id_criterio", "classificacao", "referencias_evidencias", "justificativa"],
        "properties": {
          "id_criterio": { "type": "string", "format": "uuid" },
          "classificacao": { "type": "string", "enum": ["realizado", "parcial", "nao_realizado", "nao_avaliavel"] },
          "referencias_evidencias": { "type": "array", "items": { "type": "string", "format": "uuid" } },
          "justificativa": { "type": "string", "minLength": 1 }
        }
      }
    },
    "resumo": { "type": "string", "minLength": 1 }
  }
}
```

---

**Pendências dos contratos:**
- [ ] Definir provedor/biblioteca de IA (impacta formato de chamada, streaming, tokens)
- [ ] Validar `resumo_estacao` mínimo (evitar vazamento de ficha)
- [ ] Confirmar se `InterpretadorIntencao` + `SimuladorPaciente` são chamadas separadas ou combinadas
- [ ] Definir schema `ResultadoAvaliacao` completo (referenciado no `ValidadorResposta`)
- [ ] Estabelecer versionamento de contratos (v1, v2...)

## 10. Avaliação

Somente após encerramento explícito (`Tentativa.status = concluida`):
1. Bloquear novas mensagens de atendimento (lock por `tentativa_id`).
2. Recuperar transcrição integral (`MensagemConversa` ordenados por `sequencia`) e versão exata (`VersaoEstacao`).
3. Recuperar critérios daquela versão (`CriterioAvaliacao`).
4. Solicitar avaliação estruturada ao `AvaliadorEstacao` (contrato IA).
5. Validar evidências contra mensagens reais (IDs devem existir na transcrição).
6. Persistir `Avaliacao` + `ResultadoCriterio`.
7. Apresentar relatório.

Classificações:
- `realizado`: Realizado;
- `parcial`: Parcialmente realizado;
- `nao_realizado`: Não realizado;
- `nao_avaliavel`: Não foi possível avaliar (limitação técnica ou cenário não prevê).

Não inferir execução sem evidência. Não inventar pontuação sem fórmula oficial.

### 10.1 Critérios não avaliáveis

- `CriterioAvaliacao.avaliavel = false` quando limitação técnica conhecida na versão (ex: ECG gráfico não disponível).
- `ResultadoCriterio.classificacao = nao_avaliavel` com `justificativa` citando a limitação.
- **Regra:** Não penalizar a estudante por `nao_avaliavel` (RB-039).

## 11. Resiliência, concorrência e idempotência

- Persistir mensagem antes da chamada externa (RB-029).
- Operações repetíveis devem possuir estratégia de idempotência (chave: `tentativa_id`, `sequencia_mensagem`, `tipo_operacao`).
- Evitar duplicação de resposta, ação ou liberação de exame (RB-065, RB-072).
- Falha do provedor de IA mantém tentativa recuperável (RB-071, RB-075).
- Não marcar interação concluída antes de validar e persistir a resposta.
- Tentativa interrompida (`abandonada`) pode ser retomada (`em_andamento`) enquanto dentro da janela de recuperação e versão ainda válida (RB-060, seção 3.1 regras-de-negocio.md).
- Encerramento e avaliação devem impedir execução concorrente duplicada (lock por `tentativa_id`, RB-073).
- Logs técnicos para investigação de falhas sem expor dados sensíveis (RB-074).
- Limites de consumo de IA por usuário/operação aplicados no backend (RB-069, RB-070).

### 11.1 Política de timeout, retry e idempotência (detalhamento)

Ver seção 7.1 de `regras-de-negocio.md` para princípios completos. Resumo arquitetural:

| Operação | Componente | Timeout | Retry | Idempotência (chave) | Tipo | Notas |
|---|---|---|---|---|---|---|
| Interpretação | `InterpretadorIntencao` | pendente | Manual (leitura) | `(tentativa_id, seq, 'interpretar')` | Leitura | Não altera estado |
| Liberação recurso | `GerenciadorEstadoClinico` | pendente | **Não** (escrita) | `(tentativa_id, seq, 'liberar_recurso')` | Escrita | Duplicação perigosa; status em RecursoSolicitado |
| Simulação resposta | `SimuladorPaciente` | pendente | Manual (leitura) | `(tentativa_id, seq, 'simular')` | Leitura | Não altera estado clínico |
| Avaliação | `AvaliadorEstacao` | pendente | Manual (leitura) | `(tentativa_id, 'avaliar')` | Leitura | Retentada se `falhou` |
| Persistência msg | Backend | — | — | `sequencia_mensagem` incremental | Base | Chave para todas |

**Princípios-chave:**
- Retry **não automático** para operações de escrita; reativação manual pelo usuário.
- Backend registra `(chave_idempotencia, status, resultado)` antes de executar; se `completed`, retorna resultado anterior.
- Falha/timeout → tentativa permanece no estado atual; usuário decide retry ou continuar.
- Resultado desconhecido (network partition): chave de idempotência permite reexecução segura.

**Pendências:** definir valores numéricos de timeout, parâmetros de backoff, janela de recuperação.

## 12. Interface e testes

Interface: Django Templates e Bootstrap 5. Telas: autenticação, dashboard, minhas estações, edição, simulação, avaliação, histórico e administração.

Testar isolamento, permissões, procedência, imutabilidade, duplicação, schemas, solicitações ambíguas, liberação de recursos, falhas do provedor de IA, retomada, idempotência, concorrência, encerramento, evidências e generalização dos dois casos-piloto.

## 13. Pendências antes da implementação

### Decisões técnicas (bloqueiam modelagem/código)
- [ ] **Modelagem física** confirmar tabelas, colunas, FKs, constraints, índices (ver seção 5.1)
- [ ] **Estados e transições** formais de `Estacao`, `VersaoEstacao`, `Tentativa`, `Avaliacao` (ver regras-de-negocio.md seções 2.1, 2.2, 3.1, 3.2)
- [ ] **Completude técnica mínima** — checklist estrutural aprovado (ver regras-de-negocio.md seção 2.3)
- [ ] **Processo de validação clínica** — status, responsável, fluxo, bloqueio de uso (ver regras-de-negocio.md seção 2.4)
- [ ] **Contratos da IA** — schemas de entrada/saída dos 5 componentes aprovados (ver seção 9.1)
- [ ] **Provedor/biblioteca de IA** — escolha aprovada (impacta contratos, limites, custos)
- [ ] **Política de timeout, retry, idempotência** — valores numéricos, chaves, backoff (ver seção 11.1, regras-de-negocio.md seção 7.1)
- [ ] **Janela de recuperação de tentativa** — valor (ex: 24h) e critérios de "autorizada"
- [ ] **Esquemas JSONB** — validação com equipe clínica e técnica (ver seção 5.1)

### Validações clínicas (bloqueiam habilitação de estações)
- [ ] **Síndrome Coronariana:** traçado de ECG didático validado (IAM parede inferior) para tornar C06–C08 avaliáveis; revisão da ficha interna (`03`) e rubrica (`04`) por docente habilitado
- [ ] **Tuberculose Pediátrica:** dados clínicos completos (idade, sexo, peso, sinais vitais, exame físico, vacinação, exames, condição do irmão); ficha interna, rubrica e validação clínica

### Verificações de ambiente
- [ ] Verificar versões Python/Django e dependências (requirements.txt)
- [ ] Confirmar convenções de migração e naming (UUID vs BIGSERIAL, snake_case)

## 14. Convenções de nomenclatura (padrão oficial do projeto)

Esta seção estabelece o padrão único e duradouro para todos os identificadores criados pelo projeto OSCE Trainer.

### 14.1 Convenções gerais

| Elemento | Convenção | Exemplo |
|---|---|---|
| Tabelas | `snake_case`, plural, português | `estacoes`, `tentativas` |
| Colunas | `snake_case`, português | `criado_em`, `estado_validacao_clinica` |
| Classes (Models, Services) | `PascalCase`, singular | `Estacao`, `VersaoEstacao`, `GerenciadorEstadoClinico` |
| Métodos e funções | `snake_case` | `criar_estacao()` |
| Variáveis | `snake_case` | `versao_atual` |
| Constantes | `UPPER_SNAKE_CASE` | `TEMPO_MAXIMO_RESPOSTA` |
| Módulos e pacotes | minúsculo, português | `estacoes`, `simulacoes` |
| Estados e enumerações | português, `snake_case` | `em_andamento`, `concluida` |
| Chaves JSON internas | português, `snake_case` | `id_tentativa`, `referencias_evidencias` |

**Regra:** Identificadores **não devem conter acentos ou cedilha**. Utilizar `avaliacao`, `nao_avaliavel`, `criado_em`, `versao_estacao`.

Os textos da interface e da documentação devem manter a ortografia normal do português brasileiro.

### 14.2 Mapeamento de entidades

| Entidade (legado) | Classe | Tabela |
|---|---|---|
| `User` | `Usuario` | `usuarios` |
| `Station` | `Estacao` | `estacoes` |
| `StationVersion` | `VersaoEstacao` | `versoes_estacao` |
| `ClinicalCase` | `CasoClinico` | `casos_clinicos` |
| `ChecklistCriterion` | `CriterioAvaliacao` | `criterios_avaliacao` |
| `Attempt` | `Tentativa` | `tentativas` |
| `ConversationMessage` | `MensagemConversa` | `mensagens_conversa` |
| `RequestedResource` | `RecursoSolicitado` | `recursos_solicitados` |
| `Assessment` | `Avaliacao` | `avaliacoes` |
| `CriterionAssessment` | `ResultadoCriterio` | `resultados_criterios` |

Utilize `Meta.db_table` explicitamente nos Models Django para garantir consistência.

Chaves estrangeiras em português: `estacao_id`, `tentativa_id`, `versao_estacao_id`, `proprietario_id`, `estudante_id`.

### 14.3 Componentes internos

| Componente (legado) | Componente | Tipo |
|---|---|---|
| `IntentInterpreter` | `InterpretadorIntencao` | LLM |
| `ClinicalStateManager` | `GerenciadorEstadoClinico` | Determinístico (backend) |
| `PatientSimulator` | `SimuladorPaciente` | LLM |
| `ResponseParser` | `ValidadorResposta` | Determinístico (backend) |
| `StationEvaluator` | `AvaliadorEstacao` | LLM |

Não traduza nomes oficiais de frameworks, bibliotecas, classes externas ou APIs de provedores. Ex: `AbstractUser`, `ForeignKey`, `JSON Schema`, `Django`, `PostgreSQL` permanecem inalterados.

### 14.4 Estados, classificações e tipos (valores internos)

| Domínio | Valores (snake_case português, sem acentos) |
|---|---|
| Tentativa | `em_andamento`, `concluida`, `abandonada`, `arquivada` |
| Avaliacao | `pendente`, `concluida`, `falhou` |
| Validacao clinica | `pendente`, `validada`, `rejeitada`, `precisa_revisao` |
| Classificacao de criterio | `realizado`, `parcial`, `nao_realizado`, `nao_avaliavel` |
| Tipo de origem | `criada_pelo_estudante`, `material_academico`, `adaptada`, `importada` |
| Status Estacao | `rascunho`, `publicada`, `arquivada` |
| Status VersaoEstacao | `rascunho`, `publicada`, `substituida` |
| Papel Mensagem | `estudante`, `paciente`, `sistema` |
| Tipo Recurso | `laboratorio`, `imagem`, `exame_fisico`, `ecg`, `outro` |
| Status RecursoSolicitado | `solicitado`, `liberado`, `negado`, `pendente_esclarecimento` |
| Tipo Avaliador | `ia`, `humano` |

### 14.5 Contratos JSON e JSON Schema

Propriedades em `snake_case` português. Exemplos de mapeamento:

| Legado | Novo |
|---|---|
| `attempt_id` | `id_tentativa` |
| `station_version_id` | `id_versao_estacao` |
| `message_sequence` | `sequencia_mensagem` |
| `target_resource` | `recurso_alvo` |
| `response_text` | `texto_resposta` |
| `evidence_refs` | `referencias_evidencias` |
| `clinical_validation_status` | `estado_validacao_clinica` |
| `source_type` | `tipo_origem` |
| `source_reference` | `referencia_origem` |
| `structured_by_id` | `estruturado_por_id` |
| `validated_by_id` | `validado_por_id` |
| `validated_at` | `validado_em` |
| `validation_notes` | `observacoes_validacao` |
| `version_number` | `numero_versao` |
| `created_at` | `criado_em` |
| `updated_at` | `atualizado_em` |
| `completed_at` | `concluido_em` |
| `abandoned_at` | `abandonado_em` |
| `recovery_window_expires_at` | `janela_recuperacao_expira_em` |

Quando houver integração com APIs externas, a camada de infraestrutura converte os nomes internos para o formato exigido pelo provedor. Não comprometer contratos externos para manter o português.