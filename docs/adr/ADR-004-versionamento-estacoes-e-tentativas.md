# ADR-004: Versionamento imutável de estações e tentativas

- **Status:** Proposta consolidada para o MVP
- **Versão documental:** 1.3
- **Contexto:** alterações na ficha/rubrica não podem mudar o significado de tentativas anteriores.
- **Decisão:** separar `Station`, `StationVersion`, `Attempt` e `Assessment`.
- **Alternativas:** edição no lugar, cópia integral por tentativa, event sourcing.
- **Consequências:** histórico reproduzível, com maior cuidado na publicação e armazenamento.
- **Regras:** versão utilizada não é sobrescrita; alteração relevante cria versão; tentativa preserva transcrição e recursos.
- **Restrição:** sem event sourcing no MVP.
