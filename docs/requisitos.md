# Visão do produto e requisitos

**Projeto:** OSCE Trainer  
**Versão documental:** 1.1  
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

> **Nota de numeração:** Os identificadores RF-037 a RF-051 pertenciam a uma versão anterior da especificação e não foram redefinidos nesta revisão. A sequência foi mantida a partir de RF-052 para preservar rastreabilidade com artefatos anteriores.

- **RF-052:** permitir definir origem do conteúdo.
- **RF-053:** registrar referência do material original.
- **RF-054:** registrar status de validação clínica e observações.
- **RF-055:** criar nova versão de uma estação.
- **RF-056:** preservar versões utilizadas anteriormente.
- **RF-057:** visualizar a configuração exata usada em uma tentativa.
- **RF-058:** retomar tentativa interrompida enquanto aberta e elegível.
- **RF-059:** identificar falhas/indisponibilidade da IA sem perder histórico.
- **RF-060:** duplicar estação sem compartilhar alterações entre cópia e original.

## 5. Requisitos não funcionais

- **RNF-001:** autenticação e autorização no backend.
- **RNF-002:** privacidade entre estudantes.
- **RNF-003:** ficha configurada é a fonte de verdade clínica.
- **RNF-004:** avaliação rastreável a mensagens.
- **RNF-005:** persistir mensagem antes de chamada externa.
- **RNF-006:** arquitetura em camadas com módulos coesos.
- **RNF-007:** testes automatizados para regras e permissões.
- **RNF-008:** falhas do provedor de IA não corrompem tentativa.
- **RNF-009:** interface responsiva.
- **RNF-010:** ações administrativas e mudanças relevantes rastreáveis.
- **RNF-011:** completude e validação clínica claramente distintas.
- **RNF-012:** dados ausentes/não validados não apresentados como fatos.
- **RNF-016:** limitar e monitorar consumo de IA por usuário.
- **RNF-017:** impedir duplicação por concorrência.
- **RNF-018:** manter registros técnicos suficientes para investigar falhas sem expor dados sensíveis desnecessários.
- **RNF-019:** aplicar autorização em todas as operações sobre recursos de usuários.
- **RNF-020:** validar dados clínicos estruturados por schemas antes do uso.

## 6. Critérios de aceite

Autenticação e perfis; autoria privada; procedência; versões imutáveis; simulação sem invenção; persistência; recuperação sem duplicação; encerramento explícito; avaliação com evidências; isolamento; execução dos dois casos-piloto sem regras de especialidade no código.

## 7. Rastreabilidade

Os identificadores RF-037 a RF-051 não foram redefinidos nesta revisão. A sequência RF-052 a RF-060 foi preservada conforme a proposta recebida.
