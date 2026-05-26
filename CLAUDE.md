# AI QA Toolkit - Instruções para Claude Code

Este repositório é um toolkit para apoiar análise de requisitos, geração de cenários e automação Playwright com IA.

Ele NÃO é o projeto final de automação.

---

# Regras obrigatórias

- Não criar projetos Playwright dentro deste repositório.
- Não criar as pastas `tests/`, `pages/`, `actions/`, `services/`, `fixtures/`, `helpers/` ou `data/` diretamente dentro do `AI-QA-TOOLKIT`, exceto quando a tarefa for alterar exemplos, documentação ou templates do próprio toolkit.
- Quando for necessário gerar automação, criar o projeto Playwright fora deste repositório.
- Antes de gerar automação, criar ou receber cenários de teste e confirmar se o usuário deseja seguir para automação.
- Não assumir informações ausentes nos inputs; registrar gaps, riscos, edge cases e premissas.

---

# Stack padrão

- Playwright
- JavaScript
- Node.js

Use JavaScript por padrão. Use TypeScript ou outra linguagem suportada pelo Playwright somente quando o usuário solicitar explicitamente.

---

# Arquitetura padrão

Quando o usuário não especificar arquitetura, usar Hybrid Architecture combinando:

- Page Object Model;
- Application Actions;
- Functional Abstractions.

Referência:

`architecture/architecture-guidelines.md`

---

# Workflow obrigatório

Seguir o fluxo central:

`workflows/qa-generation-flow.md`

Resumo:

0. Se os inputs recebidos forem sem texto após "My request for Code:", sempre perguntar ao usuário quais das opções deseja executar antes de prosseguir.
1. Analisar os inputs recebidos.
2. Identificar gaps, riscos, edge cases e premissas.
3. Gerar cenários de teste rastreáveis.
4. Perguntar se o usuário quer revisar os cenários antes da automação.
5. Gerar automação somente após confirmação ou solicitação explícita.
6. Aplicar a arquitetura definida ou a arquitetura padrão.
7. Entregar resumo claro do que foi gerado.

---

# Prompts oficiais

Usar os prompts abaixo como fonte principal:

- `prompts/analyze-input.md`
- `prompts/generate-scenarios.md`
- `prompts/generate-playwright-tests.md`

---

# Conduta esperada

- Ser objetivo e útil para QAs debaterem com o time.
- Separar fatos observados, gaps, riscos, premissas e perguntas.
- Manter rastreabilidade entre requisitos, cenários e automação.
- Evitar overengineering.
- Respeitar as convenções já existentes no toolkit.
