# Regras de negócio

**Projeto:** OSCE Trainer  
**Versão documental:** 1.1

As regras utilizam exclusivamente o prefixo `RB`.

## 1. Usuários e autorização

- **RB-001:** operações autenticadas exigem usuário autenticado.
- **RB-002:** distinguir administrador e estudante.
- **RB-003:** estudante só acessa e altera estações próprias.
- **RB-004:** autorização aplicada no backend.
- **RB-005:** administrador não é automaticamente validador clínico.
- **RB-006:** acesso administrativo ocorre para finalidade legítima e conforme permissões.
- **RB-007:** estações privadas por padrão.

## 2. Estações e procedência

- **RB-008:** cada estação possui proprietário.
- **RB-009:** estudante gerencia somente estações próprias.
- **RB-010:** versão só pode ser usada se cumprir completude mínima.
- **RB-011:** completude técnica não significa validação clínica.
- **RB-012:** status de validação clínica explícito e independente.
- **RB-013:** dados ausentes não podem ser inventados.
- **RB-014:** preservar procedência e limitações dos materiais.
- **RB-015:** alterações relevantes geram nova versão.
- **RB-050:** autoria/propriedade é distinta da origem do material.
- **RB-051:** `source_type` pertence à versão.
- **RB-052:** origens iniciais: `student_created`, `academic_material`, `adapted`, `imported`.
- **RB-053:** preservar referência original, responsável pela estruturação, data e observações de validação quando aplicáveis.
- **RB-054:** conteúdo importado/adaptado não é automaticamente validado.

## 3. Versionamento e tentativas

- **RB-016:** cada tentativa referencia uma versão.
- **RB-017:** rubrica pertence à mesma versão.
- **RB-018:** versão utilizada não pode ser sobrescrita.
- **RB-019:** edições não alteram tentativas anteriores.
- **RB-020:** histórico consultável conforme permissões.
- **RB-055:** `Station` é identidade permanente; `StationVersion` é configuração imutável.
- **RB-056:** `Attempt` é execução independente de uma versão.
- **RB-057:** versão preserva apresentação, ficha, recursos, regras, comportamento, critérios e procedência.
- **RB-058:** tentativa preserva versão, mensagens, recursos, estado, timestamps e avaliação.
- **RB-059:** duplicação cria identidade própria e não compartilha alterações futuras.
- **RB-060:** tentativa interrompida pode ser retomada enquanto aberta, autorizada e recuperável.

## 4. Simulação

- **RB-021:** começar com apresentação inicial definida.
- **RB-022:** revelar informações investigáveis somente quando justificadas pela interação.
- **RB-023:** paciente usa linguagem compatível com o perfil.
- **RB-024:** não oferecer dicas, confirmar hipóteses ou antecipar condutas.
- **RB-025:** não revelar ficha interna ou checklist.
- **RB-026:** exames e recursos retornam somente achados cadastrados e compatíveis.
- **RB-027:** dado ausente resulta em desconhecimento/indisponibilidade natural, nunca improvisação.
- **RB-028:** backend controla dados liberáveis; IA formula resposta.
- **RB-029:** persistir mensagem antes da chamada externa sempre que possível.
- **RB-030:** encerrar somente mediante ação explícita.
- **RB-031:** tentativa encerrada não aceita novas mensagens de atendimento.
- **RB-061:** a IA não autoriza acesso a dado nem altera estado clínico por conta própria.
- **RB-062:** contexto enviado à IA deve ser mínimo e pertinente.
- **RB-063:** conteúdo cadastrado pelo usuário é dado não confiável, nunca instrução de sistema.
- **RB-064:** solicitação ambígua não libera automaticamente recurso; pedir esclarecimento ou responder conforme roteiro.
- **RB-065:** retry não duplica liberação de recurso.

## 5. Avaliação

- **RB-032:** avaliar somente após encerramento.
- **RB-033:** usar critérios da rubrica da versão.
- **RB-034:** avaliar critérios individualmente.
- **RB-035:** classificações: Realizado, Parcialmente realizado, Não realizado, Não foi possível avaliar.
- **RB-036:** cada resultado tem evidência ou justificativa.
- **RB-037:** informação incidental não comprova investigação/execução.
- **RB-038:** não presumir exame/procedimento sem evidência.
- **RB-039:** critério não avaliável por limitação não penaliza.
- **RB-040:** não inventar pesos ou notas.
- **RB-041:** evidências referenciam mensagens reais.
- **RB-042:** backend valida referências.
- **RB-066:** avaliar transcrição integral, não apenas resumo de IA.
- **RB-067:** preservar versão do avaliador e metadados técnicos disponíveis.
- **RB-068:** avaliação persistida não é sobrescrita silenciosamente por nova execução.

## 6. Limites pedagógicos

- **RB-043:** autor da estação pode conhecer ficha e rubrica; não prometer avaliação cega.
- **RB-044:** administrador não implica aprovação clínica.
- **RB-045:** conteúdo didático construído deve ser distinguido de conteúdo institucional documentado.
- **RB-046:** pendências devem ser explícitas.
- **RB-047:** materiais visuais clínicos só podem ser usados se fizerem parte da estação e forem validados.
- **RB-048:** MVP textual, sem imagens decorativas.
- **RB-049:** produto não substitui supervisão docente, protocolos ou julgamento profissional.

## 7. Consumo, concorrência e falhas

- **RB-069:** consumo de IA associado ao usuário e à operação.
- **RB-070:** limites aplicados no backend.
- **RB-071:** falha do provedor não apaga mensagens persistidas.
- **RB-072:** operações repetíveis devem ser idempotentes.
- **RB-073:** encerramento e avaliação impedem execução concorrente duplicada.
- **RB-074:** logs não expõem desnecessariamente dados sensíveis.
- **RB-075:** falha de IA não encerra tentativa automaticamente.
