# Visão do produto e requisitos

**Projeto:** OSCE Trainer  
**Versão documental:** 1.2  
**Data:** 2026-11-15  
**Status:** proposta revisada

## 1. Visão

Aplicação web educacional para treinamento individual de estações OSCE. Estudantes criam estações de diferentes especialidades e origens, conduzem simulações textuais com pacientes padronizados apoiados por IA e recebem avaliações formativas baseadas em checklists.

## 2. Objetivos do MVP

- Cadastro e autenticação multiusuário.
- Perfis de administrador e estudante.
- Autoria e gestão de estações pelo estudante.
- Registro de origem e procedência do conteúdo.
- Versionamento imutável.
- Simulações textuais com IA.
- Controle backend das informações clínicas divulgadas.
- Avaliação baseada na transcrição e rubrica da versão utilizada.
- Histórico de tentativas.
- Isolamento por proprietário.

## 3. Atores

**Administrador:** gerencia contas, permissões e aspectos operacionais. Não é automaticamente validador clínico.

**Estudante:** cria e gerencia estações próprias, realiza tentativas e consulta histórico/avaliações.

**Validador clínico:** papel/processo separado, atribuído a profissional habilitado.

## 4. Requisitos funcionais

### Contas e autorização
- **RF-001:** permitir autenticação.
- **RF-002:** distinguir administrador e estudante.
- **RF-003:** restringir operações administrativas.
- **RF-004:** permitir gestão administrativa de contas e permissões no escopo definido.
- **RF-005:** impedir acesso não autorizado a recursos de outro estudante.

### Estações e autoria
- **RF-006:** permitir criação de estações.
- **RF-007:** permitir consultar, editar e excluir estações próprias, respeitando o histórico.
- **RF-008:** cadastrar título, área, público/faixa etária, instruções e apresentação inicial.
- **RF-009:** cadastrar ficha clínica estruturada.
- **RF-010:** cadastrar respostas condicionadas e regras de divulgação.
- **RF-011:** cadastrar exames e resultados.
- **RF-012:** cadastrar critérios avaliativos.
- **RF-013:** validar completude mínima antes de habilitar versão.
- **RF-014:** manter estações privadas por padrão.
- **RF-015:** preservar versões utilizadas em tentativas.

### Simulação
- **RF-016:** iniciar tentativa a partir de versão apta.
- **RF-017:** apresentar somente informações destinadas ao estudante.
- **RF-018:** registrar mensagens da tentativa.
- **RF-019:** registrar ações e solicitações quando houver evidência.
- **RF-020:** controlar divulgação clínica.
- **RF-021:** liberar resultados previstos e solicitados de forma compatível.
- **RF-022:** permitir encerramento explícito.
- **RF-023:** bloquear mensagens de atendimento após encerramento.
- **RF-024:** vincular tentativa à versão exata.

### Integração com IA
- **RF-025:** utilizar IA para interpretação e formulação de respostas.
- **RF-026:** enviar somente contexto necessário.
- **RF-027:** validar contratos e saídas estruturadas.
- **RF-028:** preservar mensagens em falhas do provedor de IA.
- **RF-029:** impedir invenção de dados ou resultados ausentes.

### Avaliação e histórico
- **RF-030:** avaliar tentativa encerrada com rubrica da versão.
- **RF-031:** classificar cada critério individualmente.
- **RF-032:** apresentar evidências rastreáveis.
- **RF-033:** diferenciar não realizado de não avaliável.
- **RF-034:** não gerar nota sem pesos/fórmula oficiais.
- **RF-035:** consultar tentativas e avaliações anteriores.
- **RF-036:** impedir que edições posteriores alterem o histórico.

### Procedência e versionamento

> **Nota de numeração:** Os identificadores **RF-037 a RF-051** e **RNF-013 a RNF-015** não estão definidos na versão atual da especificação. O histórico do repositório não contém versões anteriores desses documentos (a documentação foi consolidada no commit `6247687`). Não é possível determinar se esses identificadores correspondiam a requisitos removidos, renumerados ou esquecidos. A numeração atual é mantida sem preenchimento das lacunas para preservar rastreabilidade. **Pendência:** confirmar origem das lacunas com a equipe responsável pela especificação anterior.

- **RF-052:** permitir definir origem do conteúdo (`tipo_origem`).
- **RF-053:** registrar referência do material original (`referencia_origem`).
- **RF-054:** registrar status de validação clínica e observações (`estado_validacao_clinica`, `observacoes_validacao`).
- **RF-055:** criar nova versão de uma estação.
- **RF-056:** preservar versões utilizadas anteriormente (imutabilidade).
- **RF-057:** visualizar a configuração exata usada em uma tentativa.
- **RF-058:** retomar tentativa interrompida enquanto aberta e elegível.
- **RF-059:** identificar falhas/indisponibilidade da IA sem perder histórico.
- **RF-060:** duplicar estação sem compartilhar alterações entre cópia e original.

## 5. Requisitos não funcionais

- **RNF-001:** autenticação e autorização no backend.
- **RNF-002:** privacidade entre estudantes (isolamento por proprietário).
- **RNF-003:** ficha configurada é a fonte de verdade clínica (backend controla estado).
- **RNF-004:** avaliação rastreável a mensagens reais da transcrição.
- **RNF-005:** persistir mensagem antes de chamada externa (idempotência).
- **RNF-006:** arquitetura em camadas com módulos coesos.
- **RNF-007:** testes automatizados para regras e permissões.
- **RNF-008:** falhas do provedor de IA não corrompem tentativa (recuperável).
- **RNF-009:** interface responsiva.
- **RNF-010:** ações administrativas e mudanças relevantes rastreáveis (auditoria).
- **RNF-011:** completude técnica e validação clínica claramente distintas.
- **RNF-012:** dados ausentes/não validados não apresentados como fatos.
- **RNF-013 a RNF-015:** *(lacuna na numeração — ver nota na seção de Procedência e versionamento)*
- **RNF-016:** limitar e monitorar consumo de IA por usuário e por operação.
- **RNF-017:** impedir duplicação por concorrência (encerramento, avaliação, liberação de recursos).
- **RNF-018:** manter registros técnicos suficientes para investigar falhas sem expor dados sensíveis desnecessários.
- **RNF-019:** aplicar autorização em todas as operações sobre recursos de usuários.
- **RNF-020:** validar dados clínicos estruturados por schemas antes do uso.

## 6. Critérios de aceite

### 6.1 Critérios gerais (MVP)

- Autenticação e perfis (admin/estudante) funcionando
- Autoria privada: estudante só acessa estações próprias
- Procedência registrada: `tipo_origem`, referência, responsável, data por versão
- Versões imutáveis: `publicada`/`substituida` não editáveis; tentativas preservam versão exata
- Simulação sem invenção: IA não cria dados clínicos; backend controla liberação
- Persistência: mensagem salva antes de chamada externa; falha de IA não corrompe tentativa
- Recuperação sem duplicação: retomada `abandonada` → `em_andamento` com idempotência
- Encerramento explícito: apenas comando do estudante encerra; bloqueia novas mensagens
- Avaliação com evidências: cada critério tem `referencias_evidencias` para mensagens reais
- Isolamento: zero vazamento de dados entre estudantes
- Arquitetura suporta estações de diferentes especialidades **sem regras específicas no código**
- **Nota:** A execução de cada caso-piloto depende de sua documentação completa e validação clínica. Síndrome Coronariana tem limitação técnica conhecida (ECG); Tuberculose Pediátrica tem dados clínicos pendentes. Não afirmar que ambos estão executáveis atualmente.

### 6.2 Cenários de teste específicos (para orientar implementação)

| Área | Cenários de teste |
|------|-------------------|
| **Isolamento entre usuários** | Estudante A não lista/edita/visualiza estações de B; tentativas de A não aparecem para B; avaliações isoladas |
| **Controle de acesso à ficha confidencial** | Apenas backend/IA acessa `CasoClinico` completo; estudante nunca recebe ficha; rubrica não vazada durante simulação |
| **Versionamento de estações** | Editar estação `publicada` → cria nova versão; versão anterior `substituida` preservada; tentativa em versão antiga ainda acessível; nova tentativa usa versão `publicada` |
| **Persistência e retomada de tentativas** | Mensagens persistidas sequencialmente; `abandonada` após timeout → retomada dentro da janela → continua com mesmo `tentativa_id`; fora da janela → `arquivada` sem avaliação |
| **Solicitação de exames e recursos** | Solicitação ambígua → negação com motivo; recurso liberado → `RecursoSolicitado.status = liberado` com dados; retry mesma solicitação → não duplica liberação (idempotência) |
| **Respostas inválidas da IA** | `ValidadorResposta` rejeita → fallback controlado; mensagem do estudante já persistida; tentativa não corrompida; log de erro técnico |
| **Falhas de conexão e timeouts** | Timeout em `SimuladorPaciente` → tentativa `em_andamento` mantida; usuário pode retry manual; `InterpretadorIntencao` timeout → mesmo comportamento |
| **Duplicidade de mensagens e solicitações** | Mesmo `sequencia_mensagem` reenviado → idempotência retorna resultado anterior; encerramento concorrente → lock previne duplicação |
| **Avaliação baseada em evidências da conversa** | Cada `ResultadoCriterio` tem `referencias_evidencias` válidos (IDs de mensagens existentes); `nao_avaliavel` só por limitação técnica conhecida; sem notas numéricas |
| **Critérios não avaliáveis por ausência de dados** | C06–C08 coronariana → `nao_avaliavel` com justificativa "ausência de ECG gráfico validado"; não conta como falha da estudante |

> **Nota:** Critérios clínicos específicos (pesos, notas de aprovação) **não são definidos** nas fontes e não devem ser inventados. A avaliação é qualitativa (RB-035, RB-040).

## 7. Rastreabilidade

Os identificadores **RF-037 a RF-051** e **RNF-013 a RNF-015** não estão definidos na versão atual. O histórico do repositório não contém versões anteriores da documentação (consolidada no commit `6247687`). Não foi possível determinar se correspondem a requisitos removidos, renumerados ou esquecidos. As lacunas são mantidas documentadas sem preenchimento. A sequência RF-052 a RF-060 e RNF-016 a RNF-020 foi preservada conforme a proposta recebida.

**Pendência:** confirmar origem das lacunas com a equipe responsável pela especificação anterior, se aplicável.