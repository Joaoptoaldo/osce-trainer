# Rubrica Qualitativa de Treinamento — Síndrome Coronariana (Adulto)

---

## 1. Identificação e Finalidade da Rubrica

* **Título do Documento:** Rubrica Qualitativa de Treinamento e Avaliação Formativa
* **Código do Arquivo:** `04-rubrica-avaliacao.md`
* **Instituição de Origem:** Instituição de ensino superior — curso de Medicina
* **Curso:** Medicina | **Período:** 3º Período | **Disciplina:** Habilidades Médicas III (HAM 3)
* **Estação:** Síndrome Coronariana (Adulto)
* **Finalidade:** Servir de guia para a inteligência artificial avaliadora após o término da simulação interativa, fornecendo à estudante um feedback formativo, estruturado e transparente, sem atribuir notas numéricas oficiais não validadas pela instituição.

---

## 2. Natureza e Limitações do Feedback Institucional

O documento de feedback original emitido pela instituição em 17/06/2026 estabelece o valor global de **10 pontos** e enumera **dez habilidades e atitudes esperadas** (C01–C10). No entanto, o material **não** especifica:

* A distribuição fracionada dos 10 pontos por critério.
* O peso individual de cada habilidade.
* Fórmulas de cálculo de nota final.
* Critérios eliminatórios formais ou divisões percentuais entre comunicação e técnica.

Portanto, esta rubrica adota uma **abordagem qualitativa e formativa**. O objetivo é mapear evidências conversacionais, classificar o desempenho de forma descritiva e apontar oportunidades reais de melhoria, evitando notas artificiais ou punições indevidas.

---

## 3. Auditoria de Avaliabilidade dos Arquivos Anteriores

Antes de aplicar a rubrica, é fundamental verificar se os artefatos anteriores (`02-especificacao-estacao-coronariana.md` e `03-ficha-interna-paciente.md`) possuem dados suficientes para sustentar a verificação de cada critério.

| Critério | Informação Necessária | Disponível na Ficha (`03`)? | Estado de Avaliabilidade | Classificação na Rubrica |
| --- | --- | --- | --- | --- |
| **C01** | Localização ou irradiação da dor | Sim (retroesternal, braço esquerdo, mandíbula) | Dados clínicos completos | **Avaliável** |
| **C02** | Característica em aperto ou opressão | Sim ("aperto forte", "peso") | Dados clínicos completos | **Avaliável** |
| **C03** | Sintomas associados (sudorese, náuseas, dispneia) | Sim (sudorese fria, náuseas) | Dados clínicos completos | **Avaliável** |
| **C04** | Fatores de risco cardiovascular (pelo menos dois) | Sim (HAS, tabagismo ativo) | Dados clínicos completos | **Avaliável** |
| **C05** | Nomear o ECG de 12 derivações | Sim (disponível na sala) | Dado documentado no cenário | **Avaliável** |
| **C06** | Identificar supradesnivelamento do ST | Conceitualmente descrito; **sem traçado gráfico validado** | Limitação técnica (ausência de ECG gráfico) | **Não avaliável** |
| **C07** | Citar derivações com o achado (DII, DIII, aVF) | Conceitualmente descrito; **sem traçado gráfico validado** | Limitação técnica (ausência de ECG gráfico) | **Não avaliável** |
| **C08** | Topografia (parede inferior) e artéria (CD) | Conceitualmente descrito; **sem traçado gráfico validado** | Limitação técnica (ausência de ECG gráfico) | **Não avaliável** |
| **C09** | Diagnósticos diferenciais (TEP, dissecção, etc.) | Sim (informações restritas condicionadas) | Dados clínicos completos | **Avaliável** |
| **C10** | Solicitar marcadores de necrose miocárdica | Sim (troponina / CK-MB previstos) | Dados clínicos completos | **Avaliável** |

> **Nota sobre C06–C08:** A coluna "Disponível na Ficha (`03`)?" reflete se a ficha interna contém a *descrição conceitual* do achado esperado. A coluna "Estado de Avaliabilidade" explica por que, na versão textual atual do simulador, esses critérios **não podem ser avaliados**: falta o suporte gráfico do traçado de ECG. A classificação final na rubrica é **Não avaliável** por limitação técnica, não por deficiência clínica da estação.

*Nota Crítica:* Como a versão atual do chat baseia-se em interações textuais e **não dispõe** de um traçado real de eletrocardiograma gráfico devidamente validado, os critérios **C06, C07 e C08** são classificados como **Não avaliáveis** nesta versão, preservando a estudante de penalizações indevidas por limitações técnicas do sistema.

**Política adotada para o ECG (Opção A):** O simulador não fornece laudo interpretativo nem representação gráfica do traçado. Informa apenas que o eletrocardiograma está disponível na sala. C06, C07 e C08 permanecem **Não Avaliáveis** até que um traçado gráfico validado seja integrado ao ambiente de simulação. A ficha interna (`03`, §15) foi alinhada a esta política.

---

## 4. Escala Qualitativa de Classificação

Para cada critério avaliável, a IA utilizará estritamente quatro conceitos:

1. **Realizado:** A estudante demonstrou a competência de maneira explícita, correta e suficiente na conversa.
2. **Parcialmente Realizado:** Houve tentativa ou citação incompleta/imprecisa, sem abranger todos os elementos do critério.
3. **Não Realizado:** Havia oportunidade clara na anamnese, mas a estudante omitiu ou apresentou conduta incompatível.
4. **Não Avaliável:** O critério não pôde ser verificado devido à ausência de suporte técnico (ex.: falta de ECG gráfico) ou restrição do cenário.

---

## 5. Rubrica Detalhada por Critério (C01–C10)

### C01 — Localização ou Irradiação

* **Descrição Institucional:** Citar localização ou irradiação como característica de dor típica (precordial, em aperto, com possível irradiação para membro superior esquerdo, mandíbula ou dorso).
* **Competência Observada:** Investigação topográfica da dor torácica.
* **Evidências para "Realizado":** A estudante pergunta explicitamente onde a dor se localiza ou se ela irradia para o braço esquerdo, mandíbula ou costas, obtendo a resposta do paciente.
* **Desempenho Parcial:** Pergunta apenas o local genérico, sem investigar irradiações características.
* **Não Realizado:** Não questiona nem a localização nem a irradiação da dor.
* **Observação Clínica:** C01 e C02 são independentes; investigar irradiação não substitui a caracterização da dor.

### C02 — Característica da Dor

* **Descrição institucional:** Descrever a dor como opressão ou aperto.
* **Competência Observada:** Identificação do caráter anginoso da dor.
* **Evidências para "Realizado":** A estudante questiona a qualidade da dor ("é tipo aperto, queimação, pontada, peso?") e valida o perfil opressivo relatado pelo paciente.
* **Desempenho Parcial:** Pergunta como é a dor, mas aceita uma resposta vaga sem aprofundar se há sensação de peso ou opressão.
* **Não Realizado:** Não investiga a natureza ou o caráter semiológico da dor.

### C03 — Sintomas Associados

* **Descrição institucional:** Citar sintomas associados compatíveis com isquemia: sudorese fria, náusea ou dispneia.
* **Competência Observada:** Reconhecimento de manifestações neurovegetativas e respiratórias da isquemia miocárdica.
* **Evidências para "Realizado":** A estudante investiga ativamente e registra a presença de sudorese fria, náuseas ou falta de ar associada.
* **Desempenho Parcial:** Pergunta sobre sintomas associados de forma muito aberta ("sente mais alguma coisa?"), mas investiga apenas um dos sintomas sem insistir nos demais.
* **Não Realizado:** Ignora completamente a pesquisa de sintomas associados.

### C04 — Fatores de Risco Cardiovascular

* **Descrição institucional:** Citar ao menos dois fatores de risco cardiovascular: hipertensão arterial, diabetes mellitus, tabagismo, dislipidemia, obesidade ou história familiar de doença coronariana.
* **Competência Observada:** Investigação sistemática de comorbidades e hábitos de risco para doença arterial coronariana.
* **Evidências para "Realizado":** A estudante questiona o histórico do paciente e identifica ativamente pelo menos dois fatores presentes (ex.: Hipertensão e Tabagismo).
* **Desempenho Parcial:** Questiona apenas um fator de risco ou cita fatores gerais irrelevantes para o caso.
* **Não Realizado:** Não investiga antecedentes pessoais, comorbidades ou tabagismo.

### C05 — Identificação do Exame (ECG)

* **Descrição institucional:** Nomear corretamente o exame disponível como eletrocardiograma de 12 derivações.
* **Competência Observada:** Reconhecimento técnico do exame de triagem cardiológica imediata.
* **Evidências para "Realizado":** A estudante verbaliza a solicitação ou o reconhecimento do eletrocardiograma especificando o formato de 12 derivações.
* **Desempenho Parcial:** Solicita apenas "um exame de coração" ou "um teste elétrico" sem nomeá-lo tecnicamente.
* **Não Realizado:** Não menciona nem solicita o eletrocardiograma.

### C06, C07 e C08 — ECG, Derivações e Topografia (Parede Inferior / CD)

* **Status Atual na Versão Textual:** **[NÃO AVALIÁVEL]** por ausência de suporte gráfico interativo de ECG na simulação atual.
* *Critérios Originais Preservados:*
* C06: Identificar supradesnivelamento do segmento ST.
* C07: Citar pelo menos uma derivação com o achado (DII, DIII ou aVF).
* C08: Localizar topografia de parede inferior e indicar artéria coronária direita.


* **Diretriz de Avaliação:** O simulador não entrega laudo nem achados do ECG. Enquanto o ambiente de chat não integrar um visualizador gráfico de ECG validado, estes critérios não pontuarão contra nem a favor da estudante, sendo classificados como “Não Avaliável” com justificativa técnica explícita.

### C09 — Diagnósticos Diferenciais

* **Descrição institucional:** Citar um diagnóstico diferencial para dor torácica aguda: dissecção de aorta, tromboembolismo pulmonar, pericardite ou pneumotórax.
* **Competência Observada:** Raciocínio crítico de emergência frente à dor torácica.
* **Evidências para "Realizado":** A estudante verbaliza a hipótese de diagnósticos diferenciais graves (ex.: TEP ou dissecção aórtica) ao investigar o quadro.
* **Desempenho Parcial:** Menciona dúvida diagnóstica vaga, mas sem citar as entidades nosológicas de emergência exigidas.
* **Não Realizado:** Restringe a hipótese unicamente ao infarto sem demonstrar raciocínio diferencial.

### C10 — Marcadores de Necrose Miocárdica

* **Descrição institucional:** Solicitar marcadores de necrose miocárdica: troponina e/ou CK-MB.
* **Competência Observada:** Conduta laboratorial de suporte ao diagnóstico de SCA.
* **Evidências para "Realizado":** A estudante solicita explicitamente a dosagem de troponina (e/ou CK-MB).
* **Desempenho Parcial:** Solicita exames gerais de sangue (hemograma, eletrólitos) sem especificar os marcadores cardíacos.
* **Não Realizado:** Não solicita nenhum marcador laboratorial de necrose.
* *Nota Clínica Atualizada:* Conforme diretrizes clínicas modernas, a **troponina cardíaca de alta sensibilidade** é o marcador de escolha; a CK-MB não deve ser exigida como obrigatória. Contudo, para manter fidelidade ao barema institucional (**C10**), a solicitação de troponina garante o pleno atendimento ao critério.

---

## 6. Observações Complementares de Comunicação e Profissionalismo

*(Feedback formativo complementar — Sem nota numérica)*

* **Clareza e Empatia:** Avaliar se a estudante utilizou linguagem acessível, acolheu o paciente e manteve postura empática frente ao quadro de dor aguda.
* **Segurança e Urgência:** Verificar se a estudante demonstrou percepção da gravidade do quadro (reconhecimento de emergência médica e priorização de condutas rápidas).
* **Organização do Raciocínio:** Observar se a anamnese seguiu uma ordem lógica (Queixa principal $\rightarrow$ HDA $\rightarrow$ Sintomas associados $\rightarrow$ Fatores de risco $\rightarrow$ Exames).

---

## 7. Procedimento de Avaliação

1. Ao término da conversa no chat, a estudante digita o comando de encerramento da estação.
2. A IA encerra a interpretação do paciente ("Roberto Carlos") e transiciona para o perfil de avaliadora.
3. O sistema analisa o histórico do chat comparando com os critérios **C01–C10**.
4. É gerado o relatório formativo padronizado para exibição imediata à estudante.

---

## 8. Modelo de Relatório Final de Desempenho

O relatório entregue à estudante seguirá estritamente a estrutura abaixo:

```text
==================================================
RELATÓRIO FORMATIVO DE DESEMPENHO — ESTAÇÃO OSCE
Disciplina: Habilidades Médicas III (HAM 3)
Estação: Síndrome Coronariana (Adulto)
==================================================

A. RESUMO DO DESEMPENHO:
[Síntese qualitativa global da consulta realizada, destacando o engajamento e a condução geral da anamnese de dor torácica.]

B. RESULTADO POR CRITÉRIO (C01–C10):
| Código | Critério Institucional | Classificação | Evidência Observada na Conversa | Justificativa Pedagógica |
| :--- | :--- | :--- | :--- | :--- |
| C01 | Localização/Irradiação | [Realizado / Parcial / Não Realizado] | [...] | [...] |
| C02 | Característica (aperto) | [...] | [...] | [...] |
| C03 | Sintomas associados | [...] | [...] | [...] |
| C04 | Fatores de risco (≥2) | [...] | [...] | [...] |
| C05 | Nomear ECG 12 derivações | [...] | [...] | [...] |
| C06 | Identificar supra de ST | Não Avaliável | Limitação técnica do chat | Ausência de ECG gráfico na versão textual |
| C07 | Citar derivações (DII,III,aVF)| Não Avaliável | Limitação técnica do chat | Ausência de ECG gráfico na versão textual |
| C08 | Topografia e artéria provável| Não Avaliável | Limitação técnica do chat | Ausência de ECG gráfico na versão textual |
| C09 | Diagnósticos diferenciais | [...] | [...] | [...] |
| C10 | Marcadores de necrose | [...] | [...] | [...] |

C. PONTOS FORTES DEMONSTRADOS:
- [Item 1 com respaldo em fala real da estudante]
- [Item 2 com respaldo em fala real da estudante]

D. OPORTUNIDADES DE MELHORIA:
- [Item 1 com orientação prática de conduta semiológica]

E. ITENS NÃO AVALIÁVEIS NESTA VERSÃO:
- C06, C07 e C08: Requerem suporte visual gráfico de eletrocardiograma para validação segura.

F. FEEDBACK COMPLEMENTAR DE COMUNICAÇÃO E POSTURA:
- [Comentários sobre empatia, clareza e postura profissional]

G. PLANO DE REVISÃO RECOMENDADO:
- Revisão de semiologia da dor torácica aguda.
- Leitura do protocolo atualizado de Síndrome Coronariana Aguda (Ministério da Saúde / ACC-AHA).
==================================================

```

---

## 9. Regras de Justiça e Prevenção de Erros para a IA Avaliadora

* **Proibido** inventar falas ou ações que a estudante não tenha digitado.
* **Proibido** atribuir notas numéricas de 0 a 10 ou converter conceitos qualitativos em pontuações oficiais.
* **Proibido** penalizar a estudante por critérios marcados como *Não Avaliáveis* (C06, C07, C08) em decorrência da ausência de ECG gráfico no chat.
* **Proibido** revelar o gabarito ou pontuar a estudante antes do encerramento oficial da estação.

---

## 10. Pendências de Validação

1. Validação final desta rubrica pela preceptoria acadêmica da disciplina de Habilidades Médicas III.
2. Futura integração de um módulo gráfico de ECG para tornar C06, C07 e C08 plenamente avaliáveis em simulações digitais avançadas.

---

## 11. Referências

* Ministério da Saúde — *Linha de Cuidado do Infarto Agudo do Miocárdio e Protocolos de Síndromes Coronarianas Agudas*.
* ACC/AHA — *2025 Guideline for the Management of Patients With Acute Coronary Syndromes*.
* Hospital das Clínicas da UFMG / Ebserh — *Protocolo Assistencial de Síndrome Coronariana Aguda*, Versão 05.