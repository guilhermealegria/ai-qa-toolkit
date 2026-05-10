# Projeto

AI QA Toolkit para geração de cenários, análise de requisitos e automação de testes utilizando IA.

---

# Objetivo

Gerar:
- análise de inputs
- cenários de teste
- automação Playwright
- estruturas arquiteturais sustentáveis

seguindo padrões modernos de engenharia de testes.

---

# Stack oficial

- Playwright
- JavaScript
- Node.js

---

# Linguagem padrão

- Utilizar JavaScript por padrão
- Utilizar outras linguagens suportadas pelo playwright apenas quando explicitamente solicitado

---

# Arquitetura padrão

Se nenhuma arquitetura for especificada:

- Utilizar Hybrid Architecture
- Combinar:
  - Page Object Model
  - Application Actions
  - Functional Abstractions

Consultar:
`architecture/architecture-guidelines.md`

---

# Fluxo padrão de geração

Sempre seguir esta estratégia:

1. Analisar os inputs recebidos
2. Identificar gaps, riscos e edge cases
3. Gerar cenários de teste
4. Gerar automação baseada nos cenários
5. Respeitar architecture-guidelines.md
6. Aplicar convenções do projeto

---

# Inputs suportados

O projeto pode receber:

- User Stories
- Critérios de aceite
- OpenAPI
- Swagger
- Figma
- Regras de negócio
- Logs
- Payloads
- Documentação técnica

---

# Prompts oficiais

Consultar:

- `prompts/analyze-input.md`
- `prompts/generate-scenarios.md`
- `prompts/generate-playwright-tests.md`

---

# Arquitetura e convenções

Consultar:

- `architecture/architecture-guidelines.md`

---

# Estrutura padrão esperada

```text
tests/
pages/
actions/
services/
helpers/
fixtures/
data/