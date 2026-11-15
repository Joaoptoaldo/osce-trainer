# Documentação do OSCE Trainer

**Versão documental:** 1.4  
**Data:** 2026-11-15  
**Status:** base documental consolidada; pendente validação clínica e decisões técnicas para início da implementação

Esta documentação reúne visão do produto, requisitos, regras de negócio, arquitetura em camadas, decisões arquiteturais, referências de implementação e procedência dos materiais clínicos. A organização e as convenções familiares do projeto SomDiabetes são usadas como referência de código, sem substituir a arquitetura em camadas definida para este produto.

## Documentos

| Documento | Versão | Conteúdo |
|---|---|---|
| [Requisitos](requisitos.md) | 1.2 | Visão, escopo, atores, requisitos funcionais (RF-001 a RF-060) e não funcionais (RNF-001 a RNF-020) |
| [Regras de negócio](regras-de-negocio.md) | 1.5 | Regras identificadas como `RB` (estados, contratos IA, políticas de falha, rastreabilidade) |
| [Arquitetura](arquitetura.md) | 1.6 | Camadas, módulos, dependências, modelo conceitual + proposta física, IA, persistência |
| [Decisões arquiteturais](decisoes-arquiteturais.md) | 1.3 | Índice dos ADRs (ADR-001 a ADR-006) |
| [Estações clínicas](estacoes/README.md) | — | Estado, procedência e estrutura dos casos-piloto |

## Convenções

- `RF`: requisito funcional.
- `RNF`: requisito não funcional.
- `RB`: regra de negócio.
- `ADR`: registro de decisão arquitetural.
- Materiais clínicos preservam origem e limitações. **Documentação não equivale a validação clínica.**
- Classificação de rastreabilidade: `documentada`, `transcrita_por_ia`, `interpretacao`, `construida`, `pendente_validacao`.
- **Identificadores internos em português** (sem acentos): tabelas, colunas, classes, estados, enumerações, chaves JSON. Ver seção 14 de `arquitetura.md`.

## Estado das decisões

Os ADRs representam decisões **propostas para o MVP** e devem ser confirmados no repositório antes da implementação.

**Principais pendências para implementação** (ver `arquitetura.md` §13 e `regras-de-negocio.md`):
- Modelagem física confirmada (tabelas, constraints, índices, schemas JSONB)
- Estados e transições formais de `Estacao`, `VersaoEstacao`, `Tentativa`, `Avaliacao`
- Completude técnica mínima (checklist aprovado)
- Processo de validação clínica (status, responsável, fluxo)
- Contratos da IA (schemas dos 5 componentes) e provedor/biblioteca
- Política de timeout, retry, idempotência (valores numéricos)
- Validação clínica das estações-piloto (ECG coronariana; dados tuberculose pediátrica)