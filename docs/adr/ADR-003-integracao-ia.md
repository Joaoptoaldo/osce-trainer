# ADR-003: Integração IA isolada

- **Status:** Proposta consolidada para o MVP
- **Contexto:** IA interpreta linguagem natural e formula respostas, mas não controla a verdade clínica.
- **Decisão:** integração na infraestrutura, acessada por contratos internos definidos pela aplicação.
- **Alternativas:** chamadas nas Views, abstração multi-provider completa, modelo local.
- **Consequências:** menor acoplamento e melhor testabilidade, exigindo contratos e validação.
- **Restrição:** apenas um provedor de IA no MVP (sem suporte multi-provider configurável). Views não podem chamar diretamente o SDK nem concentrar a orquestração da integração; devem acionar casos de uso da aplicação.
- **Segurança:** backend decide o que pode ser revelado; contexto mínimo enviado ao provedor.
