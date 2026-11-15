# Simulador OSCE — Contexto e fontes

**Documento:** 01 — Contexto e fontes  
**Projeto:** Simulador educacional de estações OSCE por chat 
**Atualização:** 14/11/2026  
**Status:** Base documental estruturada (arquivos 01 a 04 criados); aguardando revisão de consistência, validação clínica e anexação de traçado de ECG didático validado para habilitar os testes avaliativos completos. Testes técnicos parciais do fluxo conversacional podem ocorrer antes dessa etapa, desde que os critérios dependentes do ECG sejam tratados como não avaliáveis.

---

## 1. Contexto e objetivo do projeto

O projeto consiste em um protótipo educacional de simulador de Exame Clínico Objetivo Estruturado (OSCE) implementado inicialmente por meio de um **Projeto do ChatGPT**, sem desenvolvimento de aplicação própria nesta etapa.

A proposta permite que uma estudante de Medicina pratique estações clínicas interativas com uma inteligência artificial configurada para executar as seguintes funções:

1. interpretar um paciente simulado;
2. manter uma ficha clínica interna e consistente durante toda a interação;
3. responder às perguntas da estudante sem revelar espontaneamente informações não investigadas;
4. permitir a solicitação de exames físicos e complementares;
5. não oferecer dicas nem antecipar diagnósticos;
6. encerrar a interpretação do paciente quando a estudante finalizar explicitamente a estação;
7. assumir posteriormente o papel de avaliadora;
8. comparar o desempenho da estudante com critérios previamente documentados;
9. fornecer feedback formativo baseado em evidências observadas na transcrição da conversa.

### 1.1 Estudante de referência e confirmação institucional

* **Estudante de referência:** Estudante de Medicina (identidade preservada).
* **Instituição:** Instituição de ensino superior — curso de Medicina.
* **Período e disciplina:** 3º período — Habilidades Médicas III.
* **Vínculo institucional:** Confirmado explicitamente pelo usuário que a estudante de referência estuda na mesma instituição e campus registrados nos documentos de feedback utilizados como fonte. Essa confirmação está concluída e não constitui mais pendência.

---

## 2. Estado atual e arquitetura documental do projeto

* **Primeira estação definida:** Síndrome Coronariana — Adulto.
* **Segunda estação prevista (expansão futura):** Tuberculose — Pediatria (a ser desenvolvida somente após validação, testagem e ajustes da primeira estação).

A documentação do projeto está estruturada em quatro arquivos interdependentes, nos quais este arquivo (`01`) atua como documento de entrada, rastreabilidade e referência geral, sem duplicar integralmente o conteúdo operacional dos demais:

| Arquivo | Responsabilidade | Relação com os demais documentos |
| --- | --- | --- |
| `01-contexto-e-fontes.md` | Contexto geral, procedência institucional, inventário de materiais, matriz curricular, referências clínicas, arquitetura do protótipo e limitações. | Documento-mãe de rastreabilidade; estabelece as premissas metodológicas e clínicas que governam os arquivos 02, 03 e 04. |
| `02-especificacao-estacao-coronariana.md` | Cenário institucional original, critérios esperados (C01–C10), interpretação pedagógica, limitações documentais e requisitos funcionais da primeira estação. | Traduz as fontes documentais do arquivo 01 em requisitos técnicos e pedagógicos específicos para a estação de Síndrome Coronariana. |
| `03-ficha-interna-paciente.md` | Caso clínico confidencial do paciente simulado ("Roberto Carlos de Oliveira"), respostas condicionadas, parâmetros de exame físico, exames e regras anti-vazamento. | Materializa os requisitos do arquivo 02 em um roteiro clínico reservado de uso exclusivo da IA durante a fase de simulação. |
| `04-rubrica-avaliacao.md` | Critérios qualitativos de avaliação (C01–C10), auditoria de avaliabilidade, regras de justiça, procedimento de transição e modelo de relatório formativo. | Utiliza os critérios do arquivo 02 e os limites técnicos do arquivo 03 (como a ausência de ECG gráfico) para guiar a IA na fase de avaliação. |

---

## 3. Regras de rastreabilidade e classificação da informação

Para evitar que construções didáticas ou interpretações clínicas sejam confundidas com documentos oficiais da banca examinadora, toda informação utilizada no projeto é classificada em cinco categorias:

* **Informação documentada:** Dados confirmados em materiais originais da instituição ou do currículo, bem como confirmações explícitas do usuário sobre o vínculo acadêmico.
* **Informação transcrita por IA:** Conteúdo extraído das fotografias dos feedbacks institucionais por meio de transcrição automatizada (Gemini), ainda sem auditoria visual independente das imagens originais.
* **Interpretação:** Leitura metodológica ou síntese técnica elaborada pela equipe do projeto sobre o funcionamento das estações e o alcance dos critérios.
* **Proposta didática (Construído):** Elementos fictícios ou pedagógicos criados para viabilizar a simulação (identidade do paciente, história detalhada, sinais vitais, falas condicionadas e escala qualitativa de avaliação).
* **Informação pendente de validação:** Itens que ainda dependem de revisão clínica humana, fornecimento de recursos gráficos (como o traçado de ECG) ou testagem prática.

---

## 4. Inventário de fontes acadêmicas e matriz curricular

### 4.1 Materiais de referência utilizados

| Material | Finalidade no projeto | Limitações conhecidas |
| --- | --- | --- |
| `Significado da OSCE.pdf` | Base metodológica sobre a dinâmica das estações OSCE, avaliação de competências e estruturação de checklists. | Conteúdo conhecido a partir de síntese prévia; o arquivo PDF original não foi reauditado linha a linha nesta etapa. |
| `Conteúdos da OSCE.pdf` | Mapeamento dos eixos temáticos semanais de Adulto e Pediatria para alinhamento curricular. | Apresenta a distribuição temática, mas não contém casos clínicos completos, portas de estação ou baremas pontuados. |
| `HAM 3 - Adulto.pdf` | Contextualização programática dos temas teóricos e práticos da disciplina Habilidades Médicas III (Adulto). | Ementa disciplinar; não substitui roteiros de simulação nem gabaritos de avaliação. |
| `SEMIOLOGIA PORTO.pdf` | Apoio teórico para fundamentos da anamnese e do exame físico semiológico. | Obra de referência semiológica geral; não substitui diretrizes clínicas atualizadas de emergência cardiovascular ou protocolos vigentes. |
| Fotografias dos baremas/feedbacks institucionais (`image_048bdc.jpg` e `image_048bd4.jpg`) | Fonte primária dos cenários reais e das habilidades esperadas nas estações de Síndrome Coronariana e Tuberculose. | Conhecidas via transcrição por IA; representam feedbacks descritivos pós-prova sem pesos individuais por item e sem imagens de exames anexadas. |

### 4.2 Eixos da matriz curricular (distinção entre currículo e critérios avaliados)

Os materiais programáticos (`Conteúdos da OSCE.pdf` e `HAM 3 - Adulto.pdf`) organizam a formação prática nos seguintes eixos temáticos:

* **Adulto:**
  * Semiologia geral e comunicação;
  * Sistema cardiovascular;
  * Sistema respiratório;
  * Sistema hemolinfopoiético e dermatologia.
* **Pediatria:**
  * Entrevista pediátrica, exame físico, crescimento e desenvolvimento;
  * Sistema cardiovascular;
  * Sistema respiratório;
  * Sistema hemolinfopoiético e dermatologia.

*Nota metodológica:* Deve-se manter rigorosa distinção entre o **conteúdo programático** previsto na matriz curricular, o **material didático** de apoio (livros e aulas) e os **critérios efetivamente cobrados** nos feedbacks institucionais transcritos. A presença de um tópico na matriz não autoriza sua inclusão automática como critério obrigatório na rubrica da estação.

---

## 5. Procedência e limitações dos feedbacks institucionais

Foram utilizados dois registros de feedback institucional pertencentes a uma **instituição de ensino superior — curso de Medicina**, 3º período, disciplina **Habilidades Médicas III**, ambos com data registrada de **17/06/2026** e valor global de **10 pontos**:

1. **Feedback 01 — Síndrome Coronariana (Arquivo de origem: `image_048bdc.jpg`):**
   * **Cenário transcrito:** Paciente de 58 anos chega ao pronto-socorro com dor torácica típica iniciada há 40 minutos e eletrocardiograma (ECG) já disponível na sala.
   * **Escopo:** Avalia reconhecimento clínico da dor típica, fatores de risco cardiovascular, identificação do ECG de 12 derivações, leitura de supradesnivelamento de ST em parede inferior (DII, DIII, aVF) com inferência da artéria coronária direita, diagnósticos diferenciais de dor torácica aguda e solicitação de marcadores de necrose miocárdica (critérios detalhados em `02-especificacao-estacao-coronariana.md`).

2. **Feedback 02 — Tuberculose (Arquivo de origem: `image_048bd4.jpg`):**
   * **Cenário transcrito:** Paciente pediátrico com tosse há três semanas, febre vespertina, perda de peso e contato domiciliar com pai diagnosticado com tuberculose pulmonar bacilífera.
   * **Escopo:** Avalia verbalização diagnóstica (pulmonar ou miliar), exame físico comunicado à mãe (ausculta pulmonar e palpação linfonodal), checagem de cartão vacinal/cicatriz de BCG, solicitação de radiografia de tórax (PA e perfil) e teste microbiológico, notificação compulsória e investigação/proteção do irmão comunicante. A transcrição não informa idade exata, sexo, peso, sinais vitais, exame físico completo, situação vacinal, resultados de exames ou condição clínica do irmão. O contexto sugere atendimento pediátrico, mas esses dados não devem ser inventados ou tratados como informações institucionais documentadas.

### 5.1 Limitações da transcrição e alcance documental

* **Transcrição por IA sem auditoria visual independente:** As fotografias originais dos feedbacks foram transcritas por outra IA (Gemini), que relatou legibilidade integral. Contudo, **não houve auditoria visual independente das imagens originais nesta etapa**, devendo a transcrição ser tratada sob essa ressalva.
* **Caráter descritivo e não exaustivo:** Embora representem feedbacks reais da instituição da estudante, esses documentos não devem ser assumidos como a rubrica oficial completa ou definitiva de futuras avaliações.
* **Ausência de ponderação numérica:** O valor global de 10 pontos registrado no cabeçalho não contém detalhamento de pesos por item, pontuação parcial ou critérios eliminatórios. Por essa razão, o simulador adota avaliação estritamente qualitativa (`04-rubrica-avaliacao.md`).

---

## 6. Estado da primeira estação: Síndrome Coronariana — Adulto

A estação **Síndrome Coronariana — Adulto** está definida como o caso-piloto do simulador, com sua estrutura distribuída entre os arquivos `02`, `03` e `04`:

* **Preservação do cenário institucional original [Informação transcrita por IA]:** *"Um paciente de 58 anos chega ao pronto-socorro com dor no peito típica iniciada há 40 minutos e um ECG já disponível na sala."* Nenhum dado pessoal adicional (nome, gênero ou profissão) pertence ao texto original.
* **Ficha clínica confidencial [Proposta didática]:** Documentada em `03-ficha-interna-paciente.md`, onde foram criados identidade fictícia, história detalhada, antecedentes, sinais vitais, exame físico e tabela de respostas condicionadas para viabilizar a atuação da IA.
* **Rubrica de avaliação [Proposta didática]:** Documentada em `04-rubrica-avaliacao.md`, estruturando os 10 critérios transcritos (C01–C10) em uma escala qualitativa (*Realizado*, *Parcialmente Realizado*, *Não Realizado* e *Não Avaliável*).

### 6.1 Situação crítica do Eletrocardiograma (ECG) [Informação pendente de validação]

Embora o cenário original informe que há um ECG disponível na sala e a ficha interna (`03`) descreva conceitualmente os achados pretendidos (supradesnivelamento de ST em DII, DIII e aVF), **o traçado original não foi fornecido e ainda não existe um traçado gráfico real ou didático validado anexado ao projeto**.

Enquanto essa pendência não for resolvida:
* Não se deve afirmar que o ECG da estação está pronto para uso avaliativo.
* Conforme estabelecido em `04-rubrica-avaliacao.md`, os critérios dependentes de leitura visual do traçado (**C06, C07 e C08**) permanecem classificados como **Não Avaliáveis** na versão puramente textual do protótipo, evitando penalização indevida à estudante.

---

## 7. Fundamentação clínica e alertas de segurança educacional

As recomendações clínicas abaixo orientam a revisão pedagógica do caso e do feedback, sem se misturarem ou alterarem o texto literal dos critérios institucionais transcritos:

### 7.1 Referências clínicas consideradas

* **Ministério da Saúde:** *Linha de Cuidado do Infarto Agudo do Miocárdio e Protocolos de Síndromes Coronarianas Agudas* (abrangendo avaliação e conduta na dor torácica, definição de infarto agudo do miocárdio e planejamento terapêutico no IAM). Disponível em: [Governo do Brasil — PCDT SCA](https://www.gov.br/saude/pt-br/assuntos/pcdt/l/linha-de-cuidado-do-infarto-agudo-do-miocardio-e-o-protocolo-de-sindromes-coronarianas-agudas.pd/view)
* **ACC/AHA:** *2025 ACC/AHA/ACEP/NAEMSP/SCAI Guideline for the Management of Patients With Acute Coronary Syndromes* (DOI: `10.1161/CIR.0000000000001309`). Disponível em: [AHA Journals](https://www.ahajournals.org/doi/10.1161/CIR.0000000000001309)
* **Hospital das Clínicas da UFMG / Ebserh:** *Protocolo Assistencial de Síndrome Coronariana Aguda* (Versão 05). Disponível em: [Ebserh — HC-UFMG](https://www.gov.br/hubrasil/pt-br/hospitais-universitarios/regiao-sudeste/hc-ufmg/saude/protocolos-assistenciais-hc-ufmg-ebserh/PRT_UCO_003_Sindrome_coronariana_aguda_V05.pdf)

### 7.2 Alertas clínicos obrigatórios

1. **Biomarcador preferencial:** A **troponina cardíaca** (preferencialmente de alta sensibilidade) é o biomarcador de escolha na investigação de necrose miocárdica.
2. **Papel da CK-MB:** A CK-MB não deve ser apresentada como exame obrigatório ou equivalente indispensável quando a troponina estiver disponível (mantém-se a menção no critério C10 apenas por fidelidade à transcrição do feedback institucional).
3. **Prioridade da reperfusão:** Havendo evidência clínica e eletrocardiográfica de IAM com supradesnivelamento do segmento ST (IAMCSST), a terapia de reperfusão **não deve ser atrasada** para aguardar resultados de biomarcadores.
4. **Incerteza anatômica na parede inferior:** O supradesnivelamento de ST em DII, DIII e aVF localiza o infarto na parede inferior, mas **não permite afirmar com certeza absoluta** que a artéria coronária direita seja a artéria culpada (a artéria circunflexa também pode ser responsável, dependendo da dominância coronariana).
5. **Finalidade estritamente educacional:** Este protótipo é uma ferramenta de treinamento acadêmico e **não substitui** protocolos assistenciais institucionais, supervisão médica nem avaliação docente formal.

---

## 8. Arquitetura de funcionamento do protótipo e limitações técnicas

### 8.1 Fluxo operacional em cinco fases

* **Fase 1 — Preparação:** Os documentos `01` a `04` são disponibilizados como fontes do Projeto do ChatGPT. As instruções operacionais do projeto devem definir como a IA utilizará esses documentos durante a simulação e a avaliação. A configuração final dessas instruções permanece pendente até a conclusão da revisão documental.
* **Fase 2 — Simulação:** A IA atua exclusivamente como o paciente simulado definido no arquivo `03`. Deve apresentar à estudante somente o enunciado da porta da estação e responder às informações investigadas, sem revelar espontaneamente dados internos ou critérios de avaliação. Essa separação é uma regra comportamental do protótipo, não uma garantia técnica de que a estudante não poderá acessar os arquivos reservados.
* **Fase 3 — Encerramento:** A estudante sinaliza explicitamente no chat que concluiu seu atendimento e deseja encerrar a estação.
* **Fase 4 — Avaliação:** Após o comando de encerramento, a IA abandona o papel do paciente, consulta o arquivo `04` e analisa o histórico integral da conversa para gerar o relatório formativo baseado em evidências.
* **Fase 5 — Revisão:** A estudante recebe os pontos fortes, oportunidades de melhoria e recomendações de estudo, podendo repetir a estação em nova sessão.

### 8.2 Limitação de segurança e isolamento de dados

O uso de um Projeto do ChatGPT baseia-se em instruções de comportamento (*prompting*) e arquivos anexados ao mesmo ambiente conversacional. **Essa arquitetura não oferece isolamento técnico absoluto nem avaliação cega inviolável**, pois os documentos contendo a ficha reservada (`03`) e a rubrica (`04`) coexistem na base de arquivos do projeto acessível à interface.

Para uma evolução futura do simulador que exija avaliação cega rigorosa e segurança contra vazamento de gabarito, será necessário desenvolver uma arquitetura externa que separe tecnicamente o banco de dados clínico/avaliativo da interface visual apresentada à estudante.

---

## 9. Próximas etapas (Roadmap atualizado)

A existência dos arquivos `01` a `04` representa a conclusão da **estruturação documental inicial**, mas não equivale à validação clínica ou operacional do simulador. As próximas etapas são:

1. [ ] **Revisar a consistência interna** entre os quatro documentos (`01`, `02`, `03` e `04`), sanando eventuais divergências de redação ou regras de aplicação.
2. [ ] **Validar clinicamente** a ficha interna do paciente (`03`), preferencialmente com revisão humana/docente.
3. [ ] **Obter ou produzir um ECG didático confiável e revisado** (IAM de parede inferior) para anexar à estação e habilitar a avaliação plena dos critérios C06–C08.
4. [ ] **Revisar a rubrica qualitativa** (`04`) quanto à clareza dos descritores e regras de evidência.
5. [ ] **Configurar as instruções finais** do Projeto do ChatGPT integrando os quatro arquivos.
6. [ ] **Testar a fase de simulação** verificando se a IA mantém consistência clínica e não vaza informações não investigadas.
7. [ ] **Testar a fase de avaliação** verificando se o relatório gerado respeita as evidências do chat e não atribui notas numéricas arbitrárias.
8. [ ] **Realizar um teste acompanhado com a estudante de referência** em ambiente real de estudo.
9. [ ] **Registrar falhas, alucinações ou inconsistências** observadas durante o teste prático.
10. [ ] **Ajustar o protótipo** da estação de Síndrome Coronariana antes de iniciar a construção da segunda estação (Tuberculose — Pediatria).