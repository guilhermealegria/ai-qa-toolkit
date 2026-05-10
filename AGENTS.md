# Projeto

AI QA Toolkit para geração de cenários, análise de requisitos e automação de testes utilizando IA.

Este repositório NÃO é o projeto de automação final.

Não criar projetos Playwright, pastas tests/, pages/, actions/, services/, fixtures/, helpers/ ou data/ diretamente dentro deste repositório, exceto quando a tarefa for alterar exemplos ou templates do próprio toolkit.

Projetos de automação gerados devem ser criados fora da pasta AI-QA-TOOLKIT.

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

0. Se os inputs recebidos forem sem texto após "My request for Code:" sempre perguntar quais das opções fazer
1. Analisar os inputs recebidos
2. Identificar gaps, riscos e edge cases
3. Gerar cenários de teste
4. Perguntar se o usário quer revisar os cenários criados antes de criar a automação
5. Gerar automação baseada nos cenários
6. Respeitar architecture-guidelines.md
7. Aplicar convenções do projeto

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

# Estrutura padrão esperada para projetos gerados

Esta estrutura deve ser usada apenas no projeto Playwright gerado, nunca diretamente dentro do AI-QA-TOOLKIT.

```text
tests/
pages/
actions/
services/
helpers/
fixtures/
data/
```