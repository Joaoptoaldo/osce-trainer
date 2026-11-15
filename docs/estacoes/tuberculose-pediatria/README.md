# Estação: Tuberculose Pediátrica

**Papel:** segundo caso de generalização  
**Status:** **incompleta; não habilitar para simulação** até completar dados clínicos, estruturar ficha interna, rubrica e obter validação clínica.

---

## 1. Conteúdo documentado (origem: feedback institucional transcrito por IA)

> **Fonte:** Fotografia `image_048bd4.jpg` (feedback institucional, 17/06/2026, 10 pontos) — transcrita por IA sem auditoria visual independente.

Cenário transcrito:
- Paciente pediátrico com tosse há três semanas, febre vespertina, perda de peso.
- Contato domiciliar com pai diagnosticado com tuberculose pulmonar bacilífera.
- Competências esperadas (transcritas):
  - Verbalização diagnóstica (pulmonar ou miliar).
  - Exame físico comunicado à mãe (ausculta pulmonar e palpação linfonodal).
  - Checagem de cartão vacinal/cicatriz de BCG.
  - Solicitação de radiografia de tórax (PA e perfil) e teste microbiológico.
  - Notificação compulsória.
  - Investigação/proteção do irmão comunicante.

**Limitações da transcrição:**
- Não informa: idade exata, sexo, peso, sinais vitais, exame físico completo, situação vacinal, resultados de exames, condição clínica do irmão.
- Contexto sugere atendimento pediátrico, mas esses dados **não devem ser inventados** nem tratados como informações institucionais documentadas.
- Ausência de pesos individuais, rubrica oficial, regras de eliminação.

---

## 2. Dados clínicos pendentes (não preencher por inferência)

| Campo | Status | Observação |
|-------|--------|------------|
| Idade exata | **Pendente** | Não documentado no feedback |
| Sexo | **Pendente** | Não documentado |
| Peso | **Pendente** | Necessário para dosagem/conduta |
| Sinais vitais | **Pendente** | PA, FC, FR, SatO₂, temperatura |
| Exame físico completo | **Pendente** | Ausculta, palpação, ectoscopia, etc. |
| Situação vacinal (BCG) | **Pendente** | Cicatriz, cartão vacinal |
| Resultados de exames | **Pendente** | Rx tórax, baciloscopia, PCR, etc. |
| Condição do irmão comunicante | **Pendente** | Idade, sintomas, exames, quimioprofilaxia |
| Identidade fictícia (nome) | **Pendente** | A ser construída para simulação |
| História detalhada (HDA) | **Pendente** | A ser construída |
| Antecedentes pessoais/familiares | **Pendente** | A ser construída |

---

## 3. Estrutura documental necessária (paralela à Síndrome Coronariana)

Para habilitar a estação, os seguintes arquivos devem ser criados em `docs/estacoes/tuberculose-pediatria/fontes/`:

| Arquivo | Responsabilidade | Status |
|---------|------------------|--------|
| `01-contexto-e-fontes.md` | Contexto, procedência, matriz curricular, referências, limitações | **Não criado** |
| `02-especificacao-estacao-tuberculose.md` | Cenário original, critérios institucionais transcritos, interpretação, requisitos | **Não criado** |
| `03-ficha-interna-paciente.md` | Ficha confidencial: identidade, HDA, antecedentes, sinais vitais, exame físico, respostas condicionadas, recursos, regras anti-vazamento | **Não criado** |
| `04-rubrica-avaliacao.md` | Critérios qualitativos, auditoria de avaliabilidade, escala, modelo de relatório | **Não criado** |

---

## 4. Objetivo arquitetural

Executar com a **mesma estrutura genérica** de versão, caso clínico, critérios, tentativa, mensagens, recursos e avaliação, **sem regras específicas de pediatria no código**. A especialidade altera apenas os dados (JSONB schemas, critérios, recursos), não a lógica de simulação ou avaliação.

---

## 5. Próximos passos (bloqueantes)

1. [ ] Obter/completar dados clínicos pendentes (tabela na seção 2) — **requer validação clínica/docente**.
2. [ ] Criar estrutura documental (4 arquivos acima) mantendo classificação de rastreabilidade (`documentada`, `transcrita_por_ia`, `interpretacao`, `construida`, `pendente_validacao`).
3. [ ] Validar clinicamente ficha interna e rubrica com docente habilitado.
4. [ ] Confirmar avaliabilidade de todos os critérios (sem limitação técnica análoga ao ECG da coronariana).
5. [ ] Só então: marcar estação como pronta para testes de simulação.

**Regra:** Não preencher lacunas clínicas com informações inventadas. Não marcar a estação como pronta enquanto houver pendências na seção 2 ou arquivos não criados na seção 3.
