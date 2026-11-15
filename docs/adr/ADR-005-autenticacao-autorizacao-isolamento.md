# ADR-005: Autenticação, autorização e isolamento

- **Status:** Proposta consolidada para o MVP
- **Versão documental:** 1.3
- **Contexto:** estudantes gerenciam conteúdo privado e a plataforma possui operações administrativas.
- **Decisão:** autenticação e permissões Django; definir usuário customizado antes da primeira migration se necessário; autorização por proprietário em toda operação.
- **Alternativas:** autenticação própria, ocultação de interface como controle, compartilhamento público.
- **Consequências:** segurança centralizada, exigindo testes abrangentes de autorização.
- **Restrição:** administrador não é automaticamente validador clínico; estações privadas por padrão.
