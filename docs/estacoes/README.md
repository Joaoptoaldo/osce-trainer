# Estações clínicas

Esta pasta reúne materiais de estações usados para validar o domínio genérico. Especialidade clínica altera os dados, não a estrutura fundamental do sistema.

| Estação | Papel | Situação |
|---|---|---|
| Síndrome Coronariana | Primeiro caso-piloto | Material estruturado (fontes 01–04 + prompt); **ECG gráfico e validação clínica pendentes**; C06–C08 não avaliáveis na versão textual |
| Tuberculose Pediátrica | Segundo teste de generalização | **Incompleta; não habilitar**; faltam dados clínicos essenciais (idade, sexo, peso, sinais vitais, exame físico, vacinação, exames, irmão); arquivos 01–04 não criados |

A preservação dos materiais não equivale à validação clínica.

## Classificação de rastreabilidade (padrão do projeto)

Todos os arquivos de estação devem classificar cada informação em:

| Categoria | Descrição |
|-----------|-----------|
| `documentada` | Confirmada em material original da instituição ou confirmação explícita do usuário |
| `transcrita_por_ia` | Extraída de imagens/documentos por IA, sem auditoria visual independente |
| `interpretacao` | Leitura metodológica ou síntese técnica da equipe do projeto |
| `construida` | Elementos fictícios/pedagógicos criados para viabilizar a simulação |
| `pendente_validacao` | Depende de revisão clínica humana, recurso gráfico ou testagem |

Nunca apresentar `construida` ou `interpretacao` como `documentada`.

> **Convenção:** Identificadores sem acentos (ex: `transcrita_por_ia`, `pendente_validacao`). Ver seção 14 de `arquitetura.md` para padrão completo.