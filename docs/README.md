# Documentação do OSCE Trainer

**Versão documental:** 1.3  
**Status:** proposta revisada para validação antes da implementação

Esta documentação reúne visão do produto, requisitos, regras de negócio, arquitetura em camadas, decisões arquiteturais, referências de implementação e procedência dos materiais clínicos. A organização e as convenções familiares do projeto SomDiabetes são usadas como referência de código, sem substituir a arquitetura em camadas definida para este produto.

## Documentos

| Documento | Conteúdo |
|---|---|
| [Requisitos](requisitos.md) | Visão, escopo, atores, requisitos funcionais e não funcionais |
| [Regras de negócio](regras-de-negocio.md) | Regras identificadas como `RB` |
| [Arquitetura](arquitetura.md) | Camadas, módulos, dependências, domínio, IA e persistência |
| [Decisões arquiteturais](decisoes-arquiteturais.md) | Índice dos ADRs |
| [Estações clínicas](estacoes/README.md) | Estado e procedência dos casos-piloto |

## Convenções

- `RF`: requisito funcional.
- `RNF`: requisito não funcional.
- `RB`: regra de negócio.
- `ADR`: registro de decisão arquitetural.
- Materiais clínicos preservam origem e limitações. Documentação não equivale a validação clínica.

Os ADRs representam decisões propostas para o MVP e devem ser confirmados no repositório antes da implementação.
