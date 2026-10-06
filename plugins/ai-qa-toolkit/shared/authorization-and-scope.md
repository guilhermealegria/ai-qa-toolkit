# Escopo, autorização e prontidão para implementação

Fonte única da regra que decide se e quando passar de análise ou cenários para implementação. Vale para qualquer ferramenta de automação (Playwright, k6 ou outra) e para qualquer agente.

## Autorização e escopo

- Se o pedido for somente análise, planejamento, cenários ou revisão, entregar essa etapa sem avançar para automação nem exigir uma decisão sobre ela.
- Se o usuário já solicitou explicitamente a automação ou autorizou essa transição na conversa, prosseguir dentro do escopo autorizado, sem pedir a mesma confirmação novamente.
- Se o usuário pediu para revisar os cenários antes da implementação, entregar os cenários e aguardar essa revisão, mesmo que a automação faça parte do pedido completo.
- Quando houver intenção de continuar o fluxo, mas a próxima etapa estiver indefinida, perguntar: `Você quer revisar os cenários ou seguir para a automação?`
- Aprovar cenários ou fornecer cenários já aprovados não autoriza, por si só, implementar. Interpretar a resposta conforme o pedido e a conversa: uma aprovação pode liberar uma revisão pendente de uma automação já solicitada, mas não ampliar um pedido limitado a cenários.
- Respeitar restrições posteriores do usuário e não refazer perguntas já respondidas.

### Exemplos de aplicação

| Pedido ou contexto | Próximo passo |
| --- | --- |
| “Gere apenas os cenários.” | Entregar cenários e encerrar a etapa. |
| “Gere cenários e automatize em Playwright.” | Analisar, gerar cenários e implementar, observando a prontidão abaixo. |
| “Automatize estes cenários aprovados.” | Implementar sem repetir a confirmação. |
| “Revise estes cenários aprovados.” | Revisar, sem implementar. |
| “Gere cenários e automação, mas quero revisar os cenários primeiro.” | Entregar cenários e aguardar a revisão; sua aprovação libera a implementação já solicitada. |
| “Aprovados”, após um pedido somente de cenários | Registrar a aprovação, sem iniciar automação. |
| “Não automatize ainda”, após uma autorização anterior | Respeitar a restrição mais recente. |

## Prontidão para implementar

Além da autorização acima:

- Criar ou receber cenários e analisar os inputs relevantes antes de convertê-los em código. Uma solicitação explícita de automação dispensa nova confirmação, mas não essa preparação.
- Reutilizar análises, cenários e decisões já disponíveis; não repetir etapas concluídas sem necessidade.
- Identificar lacunas que afetem comportamento esperado, destino ou critérios de validação. Esclarecer o que bloquear a implementação e avançar nas partes independentes, sem inventar requisitos.
- Se houver revisão solicitada pelo usuário ainda pendente, aguardar sua conclusão antes de implementar.
