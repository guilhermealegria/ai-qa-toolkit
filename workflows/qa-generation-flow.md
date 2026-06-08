# QA Generation Flow

Este workflow define o fluxo central do AI QA Toolkit para análise de inputs, geração de cenários e geração de automação Playwright.

Ele deve ser usado por qualquer agente ou ferramenta que trabalhe neste repositório, incluindo Codex e Claude Code.

---

# Premissas

- O repositório `AI-QA-TOOLKIT` é um toolkit, não o projeto final de automação.
- A análise deve ser objetiva e útil para QAs debaterem com o time.
- A automação deve ser gerada apenas a partir de cenários claros ou aprovados.
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

## 5. Perguntar se o usuário quer revisar os cenários

Antes de gerar automação, perguntar:

`Você quer revisar os cenários antes da automação?`

Se o usuário pedir somente análise ou somente cenários, não avançar automaticamente para automação.

---

## 6. Gerar automação Playwright quando aprovado

Gerar automação somente quando:

- o usuário aprovar os cenários;
- o usuário solicitar explicitamente a automação;
- ou já existirem cenários aprovados como entrada.

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
