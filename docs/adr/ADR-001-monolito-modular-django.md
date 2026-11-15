# ADR-001: Monólito modular com Django

- **Status:** Proposta consolidada para o MVP
- **Versão documental:** 1.3
- **Contexto:** aplicação web multiusuário com autoria, simulação e avaliação.
- **Decisão:** Django em monólito modular, organizado por camadas e módulos de domínio.
- **Alternativas:** microserviços, frontend/backend separados, framework minimalista.
- **Consequências positivas:** menor custo operacional; autenticação, ORM e administração integrados.
- **Consequências negativas:** exige disciplina para evitar acoplamento entre módulos/camadas.
- **Restrição:** sem serviços independentes no MVP.
