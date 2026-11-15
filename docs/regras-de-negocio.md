# Regras de negócio

**Projeto:** OSCE Trainer  
**Versão documental:** 1.5  
**Data:** 2026-11-15

As regras utilizam exclusivamente o prefixo `RB`.

> **Nota sobre numeração:** A numeração RB **é sequencial de RB-001 a RB-075**. As regras são definidas ao longo de todas as seções do documento (não apenas nas primeiras seções). **RB-054** e **RB-055** são consecutivos. Não renumerar identificadores já referenciados em outros documentos.

## 1. Usuários e autorização

- **RB-001:** operações autenticadas exigem usuário autenticado.
- **RB-002:** distinguir administrador e estudante.
- **RB-003:** estudante só acessa e altera estações próprias.
- **RB-004:** autorização aplicada no backend (não apenas na interface).
- **RB-005:** administrador não é automaticamente validador clínico.
- **RB-006:** acesso administrativo ocorre para finalidade legítima e conforme permissões.
- **RB-007:** estações privadas por padrão.

> **Convenção de nomenclatura:** Esta documentação utiliza nomes em português para conceitos de domínio (ex: `Estacao`, `Tentativa`, `Avaliacao`). Ver seção 14 de `arquitetura.md` para convenção completa.

## 2. Estações e procedência

- **RB-008:** cada estação possui proprietário.
- **RB-009:** estudante gerencia somente estações próprias.
- **RB-010:** versão só pode ser usada em tentativas se cumprir completude técnica mínima.
- **RB-011:** completude técnica não significa validação clínica.
- **RB-012:** status de validação clínica explícito e independente da completude técnica.
- **RB-013:** dados ausentes não podem ser inventados.
- **RB-014:** preservar procedência e limitações dos materiais.
- **RB-015:** alterações relevantes geram nova versão.
- **RB-050:** autoria/propriedade é distinta da origem do material.
- **RB-051:** `tipo_origem` pertence à versão.
- **RB-052:** origens iniciais: `criada_pelo_estudante`, `material_academico`, `adaptada`, `importada`.
- **RB-053:** preservar referência original, responsável pela estruturação, data e observações de validação quando aplicáveis.
- **RB-054:** conteúdo importado/adaptado não é automaticamente validado.

### 2.1 Estados da Estacao (identidade permanente)

> **Proposta de modelagem (sujeita a validação):** A Estacao possui estados de ciclo de vida independentes da versão e da validação clínica.

| Estado | Descrição | Transições permitidas |
|--------|-----------|----------------------|
| `rascunho` | Estação em edição, não disponível para tentativas | → `publicada` (após criar versão com completude técnica) |
| `publicada` | Estação possui ao menos uma versão publicada; disponível para o proprietário iniciar tentativas | → `arquivada` |
| `arquivada` | Estação encerrada; não aceita novas versões nem tentativas | (terminal) |

**Regras:**
- Uma Estacao em `rascunho` não possui versões `publicada`.
- A transição `rascunho` → `publicada` ocorre quando a primeira VersaoEstacao atinge completude técnica e é publicada.
- `arquivada` é irreversível no MVP.
- Validação clínica (`estado_validacao_clinica`) é atributo da **VersaoEstacao**, não da Estacao.

### 2.2 Estados da VersaoEstacao (configuração imutável)

> **Proposta de modelagem (sujeita a validação):** A VersaoEstacao é imutável após publicação. Seu estado reflete disponibilidade para tentativas.

| Estado | Descrição | Transições permitidas |
|--------|-----------|----------------------|
| `rascunho` | Versão em construção; editável pelo proprietário | → `publicada` (quando atinge completude técnica) |
| `publicada` | Versão congelada, com completude técnica; elegível para novas tentativas | → `substituida` (quando nova versão é publicada para a mesma Estacao) |
| `substituida` | Versão substituída por mais nova; não elegível para novas tentativas, mas preservada para histórico | (terminal) |

**Regras:**
- Apenas versões `publicada` podem ser usadas para iniciar tentativas (RB-010).
- Uma vez `publicada`, a versão **não pode ser editada** (RB-018, RB-055). Alterações exigem nova versão.
- `substituida` ocorre automaticamente ao publicar nova versão da mesma Estacao.
- `estado_validacao_clinica` (pendente/validada/rejeitada/precisa_revisao) é ortogonal ao estado acima.
- Versões `substituida` permanecem vinculadas a tentativas já iniciadas (RB-016, RB-017, RB-018).

### 2.3 Completude técnica mínima (checklist estrutural)

> **Proposta (sujeita a aprovação):** Uma VersaoEstacao atinge completude técnica quando possui **todos** os itens abaixo:
1. Título, área/clínica, público/faixa etária, instruções, apresentação inicial (porta da estação)
2. Ficha clínica estruturada (história, antecedentes, sinais vitais, exame físico — campos obrigatórios por schema)
3. Pelo menos **um** critério avaliativo (CriterioAvaliacao)
4. Regras de divulgação configuradas (mesmo que vazias/permissivas)
5. Recursos/exames: **opcionais** globalmente; obrigatórios apenas se o cenário clínico exigir (definido na ficha)

**Pendência:** confirmar checklist com a equipe. A regra "recursos obrigatórios apenas quando o cenário exigir" evita forçar exames em estações que não os necessitam.

### 2.4 Validação clínica

> **Proposta (sujeita a aprovação):** Validação clínica é processo **separado** da completude técnica, aplicado à VersaoEstacao.

- **Status possíveis:** `pendente` (padrão ao publicar), `validada`, `rejeitada`, `precisa_revisao`.
- **Registro obrigatório quando validada:** `validado_por_id` (usuário), `validado_em` (timestamp), `validado_versao_id` (FK para VersaoEstacao), `observacoes_validacao` (observações).

**Regras de uso por status:**

| `estado_validacao_clinica` | Pode iniciar novas tentativas? | Apresentação para estudante |
|----------------------------|--------------------------------|------------------------------|
| `pendente` | **Sim** (RB-011: completude ≠ validação) | "Validação clínica pendente" — não apresentar como aprovada |
| `validada` | Sim | "Validada clinicamente em [data] por [validador]" |
| `rejeitada` | **Não** (bloqueada para novas tentativas) | Não ofertar para novas tentativas |
| `precisa_revisao` | **Não** (bloqueada para novas tentativas) | Não ofertar para novas tentativas |

**Diferenciação por origem (`tipo_origem`):**
- `criada_pelo_estudante` (estações criadas para estudo próprio): validação clínica **opcional**; `pendente` é padrão aceitável.
- `material_academico`, `adaptada`, `importada` (conteúdo institucional/externo): validação clínica **esperada**; `pendente` indica "aguardando validação", não "aprovado".

**Regra:** Uma versão `rejeitada` ou `precisa_revisao` **nunca** deve ser apresentada como clinicamente aprovada. Tentativas já iniciadas em versões que depois mudam status são preservadas (RB-018).

**Pendências:**
- Definir quem pode validar (papel/permissão: `validador_clinico`).
- Definir fluxo de solicitação de validação.
- Confirmar bloqueio de `rejeitada`/`precisa_revisao` para novas tentativas (recomendado: sim).

## 3. Versionamento e tentativas

- **RB-016:** cada tentativa referencia uma versão.
- **RB-017:** rubrica pertence à mesma versão.
- **RB-018:** versão utilizada não pode ser sobrescrita.
- **RB-019:** edições não alteram tentativas anteriores.
- **RB-020:** histórico consultável conforme permissões.
- **RB-055:** `Estacao` é identidade permanente; `VersaoEstacao` é configuração imutável.
- **RB-056:** `Tentativa` é execução independente de uma versão.
- **RB-057:** versão preserva apresentação, ficha, recursos, regras, comportamento, critérios e procedência.
- **RB-058:** tentativa preserva versão, mensagens, recursos, estado, timestamps e avaliação.
- **RB-059:** duplicação cria identidade própria e não compartilha alterações futuras.
- **RB-060:** tentativa interrompida pode ser retomada enquanto aberta, autorizada e recuperável.

### 3.1 Estados da Tentativa (execução da simulação)

> **Proposta de modelagem (sujeita a validação):**

| Estado | Descrição | Transições permitidas |
|--------|-----------|----------------------|
| `em_andamento` | Tentativa ativa; aceita mensagens de atendimento, solicitações de recursos, comandos de encerramento | → `concluida` (encerramento explícito), → `abandonada` (expiração/abandono) |
| `concluida` | Estudante encerrou explicitamente (`ENCERRAR ESTAÇÃO`); aguarda ou possui avaliação | (não há transição de estado da Tentativa pela avaliação) |
| `abandonada` | Tentativa interrompida sem encerramento explícito (ex: timeout de sessão, usuário saiu); elegível para retomada se dentro de janela de recuperação | → `em_andamento` (retomada autorizada), → `arquivada` (janela expirada) |
| `arquivada` | Tentativa finalizada sem avaliação (janela de retomada expirada); preservada para histórico | (terminal) |

**Regras:**
- Apenas `em_andamento` aceita novas mensagens de atendimento (RB-031).
- `concluida` bloqueia novas mensagens de atendimento; permite avaliação (RB-032).
- A avaliação **não altera o estado da Tentativa** — Avaliacao possui ciclo de vida independente (ver seção 3.2).
- **Retomada (RB-060):** permitida apenas para `abandonada` → `em_andamento` se: (a) tentativa não foi avaliada, (b) versão ainda existe e está `publicada` ou `substituida`, (c) usuário proprietário, (d) dentro de janela de recuperação (**pendente: definir valor**).
- Tentativa `abandonada` fora da janela vira `arquivada` (terminal, sem avaliação).
- **Pendência:** definir "janela de recuperação" e critério de "autorizada" (tempo? permissões? versão ainda válida?).

### 3.2 Estados da Avaliacao (avaliação)

> **Proposta de modelagem (sujeita a validação):** Ciclo de vida separado da Tentativa.

| Estado | Descrição | Transições permitidas |
|--------|-----------|----------------------|
| `pendente` | Avaliação solicitada/em processamento (ex: aguardando IA) | → `concluida`, → `falhou` |
| `concluida` | Avaliação finalizada com sucesso; resultados persistidos | (terminal) |
| `falhou` | Falha na geração da avaliação (ex: erro de IA, timeout); Tentativa permanece `concluida` e pode ser reavaliada | → `pendente` (retentar) |

**Regras:**
- Avaliação só ocorre após `Tentativa.status = concluida` (RB-032).
- Avaliação usa rubrica da VersaoEstacao referenciada pela Tentativa (RB-033).
- Avaliação `falhou` não bloqueia a Tentativa; permite nova tentativa de avaliação (RB-075).
- Avaliação `concluida` é imutável (RB-068).
- **Pendência:** definir se avaliação pode ser refeita após `concluida` (ex: contestação) — MVP sugere não.

## 4. Simulação

- **RB-021:** começar com apresentação inicial definida (porta da estação).
- **RB-022:** revelar informações investigáveis somente quando justificadas pela interação.
- **RB-023:** paciente usa linguagem compatível com o perfil.
- **RB-024:** não oferecer dicas, confirmar hipóteses ou antecipar condutas.
- **RB-025:** não revelar ficha interna ou checklist.
- **RB-026:** exames e recursos retornam somente achados cadastrados e compatíveis.
- **RB-027:** dado ausente resulta em desconhecimento/indisponibilidade natural, nunca improvisação.
- **RB-028:** backend controla dados liberáveis; IA formula resposta.
- **RB-029:** persistir mensagem antes da chamada externa sempre que possível (idempotência).
- **RB-030:** encerrar somente mediante ação explícita do estudante.
- **RB-031:** tentativa encerrada não aceita novas mensagens de atendimento.
- **RB-061:** a IA não autoriza acesso a dado nem altera estado clínico por conta própria.
- **RB-062:** contexto enviado à IA deve ser mínimo e pertinente.
- **RB-063:** conteúdo cadastrado pelo usuário é dado não confiável, nunca instrução de sistema.
- **RB-064:** solicitação ambígua não libera automaticamente recurso; pedir esclarecimento ou responder conforme roteiro.
- **RB-065:** retry não duplica liberação de recurso.

### 4.1 Responsabilidades da integração com IA (contratos)

> **Proposta de contratos (sujeita a aprovação e definição de provedor):**

A integração é isolada na camada de infraestrutura atrás de contratos internos definidos pela aplicação (ADR-003). O backend **não** envia a ficha integral à IA em toda interação.

**Distinção fundamental:**

| Tipo | Componentes | Característica |
|------|-------------|----------------|
| **Usam LLM** (probabilísticos) | `InterpretadorIntencao`, `SimuladorPaciente`, `AvaliadorEstacao` | Requerem validação de saída (`ValidadorResposta`) |
| **Determinísticos** (backend) | `GerenciadorEstadoClinico`, `ValidadorResposta` | Autoridade para permissões, liberação, validação de formato |

> **Regras críticas:**
> - A IA **não** decide permissões, não altera estados, não autoriza recursos.
> - `GerenciadorEstadoClinico` é a autoridade determinística para liberação de dados.
> - `ValidadorResposta` valida formato/campos **antes** de persistir qualquer resposta da IA.
> - Encerramento da tentativa depende de ação explícita da estudante, validada pelo backend (não pela IA).

**Componentes lógicos e contratos propostos (schemas formais em `arquitetura.md` §9.1):**

| Componente | Tipo | Responsabilidade |
|------------|------|------------------|
| `InterpretadorIntencao` | LLM | Interpretar mensagem livre em intenção estruturada (pergunta, solicitação de exame, hipótese, encerramento) |
| `GerenciadorEstadoClinico` | Backend (determinístico) | Verificar validade da ação, disponibilidade do dado, regras de divulgação; **decidir liberação** |
| `SimuladorPaciente` | LLM | Formular resposta natural do paciente **limitada aos dados autorizados** pelo GerenciadorEstadoClinico |
| `ValidadorResposta` | Backend (determinístico) | Validar formato e campos da resposta estruturada da IA **antes de persistir** |
| `AvaliadorEstacao` | LLM | Avaliar tentativa encerrada segundo rubrica da versão (somente após `Tentativa.concluida`) |

**Pendências:**
- Definir provedor/biblioteca de IA (impacta formatos de API, streaming, tokens)
- Validar schemas JSON Schema com equipe técnica (`arquitetura.md` §9.1)
- Definir `resumo_estacao` mínimo no contexto (evitar vazamento de ficha)
- Confirmar se `InterpretadorIntencao` e `SimuladorPaciente` são chamadas separadas ou combinadas

### 4.2 Validação de respostas da IA

- Toda resposta estruturada da IA **deve** passar por `ValidadorResposta` antes de persistir (RB-027, RB-061).
- Resposta inválida ou incompleta → **não persistir como ação do paciente**; registrar erro técnico; retornar fallback controlado (ex: "O paciente não consegue responder no momento") ou retry.
- IA **nunca** pode: inventar dados clínicos, confirmar hipóteses, liberar recursos não autorizados, alterar estado da tentativa.
- Backend valida referências de evidências na avaliação (RB-042).

### 4.3 Tratamento de falhas e recuperação

- **Timeout da IA:** configurável (**pendente: valor**); em timeout, mensagem do estudante já persistida (RB-029); tentativa permanece `em_andamento`; retry opcional.
- **Retry:** **não automático** quando não for possível determinar se operação anterior foi processada (ex: liberação de recurso). Chave de idempotência: `(tentativa_id, sequencia_mensagem, tipo_operacao)` (**pendente: confirmar**).
- **Falha do provedor:** não apaga mensagens persistidas (RB-071); tentativa recuperável (RB-075); log técnico para investigação (RB-074).
- **Recuperação de tentativa:** `abandonada` → `em_andamento` dentro de janela (RB-060, seção 3.1).

## 5. Avaliação

- **RB-032:** avaliar somente após encerramento explícito (`Tentativa.concluida`).
- **RB-033:** usar critérios da rubrica da versão referenciada pela tentativa.
- **RB-034:** avaliar critérios individualmente.
- **RB-035:** classificações: `realizado` (Realizado), `parcial` (Parcialmente realizado), `nao_realizado` (Não realizado), `nao_avaliavel` (Não foi possível avaliar).
- **RB-036:** cada resultado tem evidência (referência a mensagem) ou justificativa.
- **RB-037:** informação incidental não comprova investigação/execução.
- **RB-038:** não presumir exame/procedimento sem evidência na transcrição.
- **RB-039:** critério não avaliável por limitação técnica (ex: ausência de ECG) não penaliza.
- **RB-040:** não inventar pesos ou notas numéricas sem fórmula oficial.
- **RB-041:** evidências referenciam mensagens reais (IDs de MensagemConversa).
- **RB-042:** backend valida referências de evidências antes de persistir avaliação.
- **RB-066:** avaliar transcrição integral, não apenas resumo de IA.
- **RB-067:** preservar versão do avaliador (IA/humano) e metadados técnicos disponíveis.
- **RB-068:** avaliação persistida não é sobrescrita silenciosamente por nova execução.

### 5.1 Critérios não avaliáveis

- Classificação `nao_avaliavel` aplica-se quando: (a) limitação técnica do sistema impede verificação (ex: ECG gráfico não disponível), (b) cenário não prevê a competência, (c) dado necessário não existe na ficha da versão.
- **Não** se usa `nao_avaliavel` por "estudante não fez" — isso é `nao_realizado`.
- Exemplo documentado: Síndrome Coronariana C06–C08 (ausência de ECG gráfico validado).

## 6. Limites pedagógicos

- **RB-043:** autor da estação pode conhecer ficha e rubrica; não prometer avaliação cega.
- **RB-044:** administrador não implica aprovação clínica.
- **RB-045:** conteúdo didático construído deve ser distinguido de conteúdo institucional documentado.
- **RB-046:** pendências devem ser explícitas.
- **RB-047:** materiais visuais clínicos só podem ser usados se fizerem parte da estação e forem validados.
- **RB-048:** MVP textual, sem imagens decorativas.
- **RB-049:** produto não substitui supervisão docente, protocolos ou julgamento profissional.

### 6.1 Classificação de rastreabilidade da informação (padrão do projeto)

Toda informação clínica nos documentos de estação deve ser classificada em uma das categorias:

| Categoria | Descrição | Exemplo |
|-----------|-----------|---------|
| `documentada` | Confirmada em material original da instituição ou confirmação explícita do usuário | Idade 58 anos, dor há 40 min (feedback institucional) |
| `transcrita_por_ia` | Extraída de imagens/documentos por IA, sem auditoria visual independente | Critérios C01–C10 transcritos de fotografias |
| `interpretacao` | Leitura metodológica ou síntese técnica da equipe do projeto | Topografia parede inferior → artéria coronária direita provável |
| `construida` | Elementos fictícios/pedagógicos criados para viabilizar a simulação | Nome "Roberto Carlos", sinais vitais, história detalhada |
| `pendente_validacao` | Depende de revisão clínica humana, recurso gráfico ou testagem | Traçado de ECG, validação da ficha por docente |

**Regra:** Nunca apresentar `construida` ou `interpretacao` como `documentada`. Manter classificação explícita nos arquivos de estação.

> **Convenção:** Identificadores sem acentos (ex: `transcrita_por_ia`, `pendente_validacao`).

## 7. Consumo, concorrência e falhas

- **RB-069:** consumo de IA associado ao usuário e à operação.
- **RB-070:** limites aplicados no backend (por usuário, por período, por operação).
- **RB-071:** falha do provedor não apaga mensagens persistidas.
- **RB-072:** operações repetíveis devem ser idempotentes (chave: `tentativa_id`, `sequencia_mensagem`, `tipo_operacao`).
- **RB-073:** encerramento e avaliação impedem execução concorrente duplicada (lock por `tentativa_id`).
- **RB-074:** logs não expõem desnecessariamente dados sensíveis (ficha clínica, transcrição).
- **RB-075:** falha de IA não encerra tentativa automaticamente (tentativa permanece recuperável).

### 7.1 Política de timeout, retry e idempotência (princípios)

> **Princípios de segurança (valores numéricos pendentes de definição):**

1. **Timeout de chamada à IA:** configurável por operação (interpretação, simulação, avaliação). Valor padrão: **pendente**.

2. **Retry (reativação manual, não automática para operações de escrita):**
   - **Não automático** para operações de liberação de recurso ou alteração de estado (`GerenciadorEstadoClinico.liberar_recurso`, encerramento, avaliação) — risco de duplicação se resultado anterior desconhecido.
   - Permitido com chave de idempotência para operações de leitura/processamento (`InterpretadorIntencao`, `SimuladorPaciente`, `AvaliadorEstacao`).
   - Backoff exponencial com jitter (**pendente: parâmetros**).
   - Reativação **manual pelo usuário** (não automática) quando operação falha ou timeout.

3. **Idempotência — chave composta por operação:**
   | Operação | Componente | Chave de idempotência | Tipo |
   |----------|------------|----------------------|------|
   | Interpretação de intenção | `InterpretadorIntencao` | `(tentativa_id, sequencia_mensagem, 'interpretar')` | Leitura |
   | Formulação resposta paciente | `SimuladorPaciente` | `(tentativa_id, sequencia_mensagem, 'simular')` | Leitura |
   | Liberação de recurso | `GerenciadorEstadoClinico` | `(tentativa_id, sequencia_mensagem, 'liberar_recurso')` | Escrita |
   | Avaliação da tentativa | `AvaliadorEstacao` | `(tentativa_id, 'avaliar')` | Leitura |
   | Persistência mensagem | Backend | `sequencia_mensagem` incremental por `tentativa_id` | Base |

   - Backend registra `(chave_idempotencia, status, resultado)` antes de executar; se chave existe com status `completed`, retorna resultado anterior.
   - Mensagem do estudante persistida **antes** da chamada externa (RB-029) com `sequencia_mensagem` incremental.

4. **Recuperação após falhas:**
   - Falha de IA (timeout, erro, resposta inválida) → tentativa permanece no estado atual (`em_andamento` ou `concluida` aguardando avaliação).
   - Usuário pode solicitar retry manual (reprocessar última mensagem) ou continuar.
   - Avaliação `falhou` → pode ser retentada (RB-075, seção 3.2).
   - Resultado desconhecido (ex: network partition): não assumir sucesso; chave de idempotência permite reexecutar com segurança.

5. **Prevenção de duplicidade:**
   - Lock otimista/pessimista em `Tentativa` para encerramento e avaliação (RB-073).
   - Liberação de recurso registrada em `RecursoSolicitado` com status; retry não duplica (RB-065).
   - `chave_idempotencia` única em `recursos_solicitados`.