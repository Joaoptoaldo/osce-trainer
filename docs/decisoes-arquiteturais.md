# Registro de decisões arquiteturais

**Versão documental:** 1.3  
**Data:** 2026-11-15

| ADR | Decisão | Estado |
|---|---|---|
| [ADR-001](adr/ADR-001-monolito-modular-django.md) | Monólito modular Django | Proposta consolidada |
| [ADR-002](adr/ADR-002-postgresql-persistencia-hibrida.md) | PostgreSQL e persistência híbrida | Proposta consolidada |
| [ADR-003](adr/ADR-003-integracao-ia.md) | Integração com IA isolada por contrato | Proposta consolidada |
| [ADR-004](adr/ADR-004-versionamento-estacoes-e-tentativas.md) | Versionamento imutável | Proposta consolidada |
| [ADR-005](adr/ADR-005-autenticacao-autorizacao-isolamento.md) | Perfis e isolamento | Proposta consolidada |
| [ADR-006](adr/ADR-006-arquitetura-em-camadas.md) | Arquitetura em camadas | Proposta consolidada |

## Princípios

Estudante é proprietário; origem é distinta de autoria; administrador não é validador clínico; domínio independente de especialidade; backend controla estado clínico; IA não é fonte de verdade; tentativa preserva versão e transcrição; autorização ocorre no backend.
