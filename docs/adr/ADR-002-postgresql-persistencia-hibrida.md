# ADR-002: PostgreSQL e persistência híbrida

- **Status:** Proposta consolidada para o MVP
- **Versão documental:** 1.3
- **Contexto:** histórico e autorização exigem integridade relacional; fichas variam por especialidade.
- **Decisão:** PostgreSQL. Entidades/relacionamentos em tabelas; blocos clínicos variáveis em JSONB com schemas validados.
- **Alternativas:** JSON integral, relacional sem flexibilidade, múltiplos bancos.
- **Consequências:** flexibilidade com integridade, ao custo de validar e versionar schemas.
- **Restrição:** critérios, tentativas e mensagens permanecem entidades próprias.
