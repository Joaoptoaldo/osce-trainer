# ADR-006: Arquitetura em camadas

- **Status:** Proposta consolidada para o MVP
- **Versão documental:** 1.3
- **Contexto:** concentrar modelos, regras, integrações e interface em `apps/` mistura responsabilidades técnicas.
- **Decisão:** organizar código em `domain`, `application`, `infrastructure`, `presentation` e `config`. Módulos de domínio representam áreas do produto; não criar apps Django para cada responsabilidade técnica.
- **Direção:** apresentação depende da aplicação; aplicação depende do domínio e de contratos; infraestrutura implementa integrações externas.
- **Decisão pragmática:** módulos de domínio podem ser Django apps e conter modelos ORM/migrations. Não duplicar modelos ORM e entidades puras sem necessidade demonstrada.
- **Alternativas:** concentrar tudo em `apps/`; Clean Architecture estrita; arquitetura hexagonal integral.
- **Consequências positivas:** responsabilidades claras e integração externa isolada.
- **Consequências negativas:** mais diretórios e disciplina de dependências.
- **Referência de implementação:** aproveitar convenções e padrões familiares do SomDiabetes, adaptando-os às camadas definidas aqui. A referência não autoriza copiar acoplamentos entre Views e serviços externos.
- **Restrição:** evitar camadas vazias, abstrações prematuras e duplicação.
