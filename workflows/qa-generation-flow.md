# QA Generation Flow

Este workflow define o fluxo central do AI QA Toolkit para análise de inputs, geração de cenários e geração de automação Playwright.

Ele deve ser usado por qualquer agente ou ferramenta que trabalhe neste repositório, incluindo Codex e Claude Code.

---

# Premissas

- O repositório `AI-QA-TOOLKIT` é um toolkit, não o projeto final de automação.
- A análise deve ser objetiva e útil para QAs debaterem com o time.
- A automação deve estar no escopo solicitado ou autorizado e partir de cenários claros, seguindo as condições das etapas 5 e 6.
- A regra de criar projeto Playwright fora do toolkit pertence à etapa de automação e está detalhada em `prompts/generate-playwright-tests.md`.
- Inputs médios ou grandes devem ser armazenados preferencialmente em `inputs/` para permitir leitura seletiva, buscas por trechos relevantes e menor uso de tokens.
- O conteúdo real de `inputs/` deve ser tratado como temporário e não versionado por padrão.

---

# Fluxo principal

## 0. Tratar request vazio

Se os inputs recebidos forem sem texto após "My request for Code:", sempre perguntar ao usuário quais das opções deseja executar antes de prosseguir.

Não avançar para análise, geração de cenários ou automação sem uma intenção mínima do usuário.

---

## 1. Analisar os inputs recebidos

Quando os inputs estiverem em arquivos locais, usar preferencialmente caminhos dentro de `inputs/`.

Exemplos:

```text
inputs/user-story-login.md
inputs/swagger-pedidos.yaml
inputs/log-erro-checkout.txt
```

Ler apenas o necessário para a análise, fazendo buscas e inspeção localizada quando o arquivo for grande.

Identificar o tipo de input recebido:

- User Story;
- critérios de aceite;
- OpenAPI ou Swagger;
- Figma ou descrição de UI;
- regras de negócio;
- logs;
- payloads;
- documentação técnica;
- combinação de múltiplas fontes.

Usar como referência:

`prompts/analyze-input.md`

---

## 2. Identificar gaps, riscos, edge cases e premissas

Separar claramente:

- fatos observados no input;
- gaps de informação;
- riscos funcionais, técnicos ou de testabilidade;
- edge cases;
- premissas usadas;
- perguntas para o time.

Não inventar comportamento ausente no input.

---

## 3. Definir Test Repository do Xray

Antes de gerar arquivos para upload no Xray, identificar se o usuário informou um destino para o Test Repository.

Seguir a regra detalhada definida em:

`prompts/generate-scenarios.md`

---

## 4. Gerar cenários de teste rastreáveis

Gerar cenários cobrindo:

- fluxos positivos;
- fluxos negativos;
- validações de contrato;
- regras de negócio;
- permissões e autenticação, quando aplicável;
- falhas externas, indisponibilidade, timeouts e conflitos;
- edge cases relevantes.

Usar como referência:

`prompts/generate-scenarios.md`

---

## 5. Verificar o escopo e a autorização para automação

Esta seção é a referência central para a transição de cenários para implementação, inclusive nos prompts de Playwright e k6.

- Se o pedido for somente análise, planejamento, cenários ou revisão, entregar essa etapa sem avançar para automação nem exigir uma decisão sobre ela.
- Se o usuário já solicitou explicitamente a automação ou autorizou essa transição na conversa, prosseguir dentro do escopo autorizado, sem pedir a mesma confirmação novamente.
- Se o usuário pediu para revisar os cenários antes da implementação, entregar os cenários e aguardar essa revisão, mesmo que a automação faça parte do pedido completo.
- Quando houver intenção de continuar o fluxo, mas a próxima etapa estiver indefinida, perguntar: `Você quer revisar os cenários ou seguir para a automação?`
- Aprovar cenários ou fornecer cenários já aprovados não autoriza, por si só, implementar. Interpretar a resposta conforme o pedido e a conversa: uma aprovação pode liberar uma revisão pendente de uma automação já solicitada, mas não ampliar um pedido limitado a cenários.
- Respeitar restrições posteriores do usuário e não refazer perguntas já respondidas.

Exemplos de aplicação:

| Pedido ou contexto | Próximo passo |
| --- | --- |
| “Gere apenas os cenários.” | Entregar cenários e encerrar a etapa. |
| “Gere cenários e automatize em Playwright.” | Analisar, gerar cenários e implementar, observando a prontidão da etapa 6. |
| “Automatize estes cenários aprovados.” | Implementar sem repetir a confirmação. |
| “Revise estes cenários aprovados.” | Revisar, sem implementar. |
| “Gere cenários e automação, mas quero revisar os cenários primeiro.” | Entregar cenários e aguardar a revisão; sua aprovação libera a implementação já solicitada. |
| “Aprovados”, após um pedido somente de cenários | Registrar a aprovação, sem iniciar automação. |
| “Não automatize ainda”, após uma autorização anterior | Respeitar a restrição mais recente. |

---

## 6. Gerar automação Playwright quando autorizada e pronta

Além da autorização definida na etapa 5:

- Criar ou receber cenários e analisar os inputs relevantes antes de convertê-los em código. Uma solicitação explícita de automação dispensa nova confirmação, mas não essa preparação.
- Reutilizar análises, cenários e decisões já disponíveis; não repetir etapas concluídas sem necessidade.
- Identificar lacunas que afetem comportamento esperado, destino ou critérios de validação. Esclarecer o que bloquear a implementação e avançar nas partes independentes, sem inventar requisitos.
- Se houver revisão solicitada pelo usuário ainda pendente, aguardar sua conclusão antes de implementar.

Usar como referência:

`prompts/generate-playwright-tests.md`

---

## 7. Aplicar arquitetura e convenções

Se o usuário não especificar arquitetura, aplicar Hybrid Architecture:

- Page Object Model;
- Application Actions;
- Functional Abstractions.

Usar como referência:

`architecture/architecture-guidelines.md`

---

## 8. Entregar resultado de forma clara

Ao finalizar, informar:

- o que foi analisado;
- principais gaps e riscos;
- cenários ou arquivos gerados;
- cobertura principal;
- arquitetura usada, quando houver automação;
- próximos passos recomendados.
