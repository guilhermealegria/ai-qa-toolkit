# Projeto

AI QA Toolkit para geração de análise de requisitos, cenários de teste e automação Playwright com apoio de IA.

Este repositório é um toolkit de apoio para QA. Ele NÃO é o projeto final de automação.

---

# Regras obrigatórias do repositório

- Não criar projetos Playwright diretamente dentro deste repositório.
- Não criar as pastas `tests/`, `pages/`, `actions/`, `services/`, `fixtures/`, `helpers/` ou `data/` dentro deste repositório, exceto quando a tarefa for alterar exemplos, documentação ou templates do próprio toolkit.
- Projetos de automação gerados devem ser criados fora da pasta `AI-QA-TOOLKIT`.
- Usar este repositório como fonte de prompts, workflows, padrões arquiteturais e convenções.
- Arquivos de entrada recebidos do usuário devem ser colocados preferencialmente em `inputs/` para reduzir uso de tokens, permitindo leitura localizada e busca por trechos relevantes.
- O conteúdo real de `inputs/` não deve ser versionado por padrão, pois pode conter documentação interna, dados sensíveis ou massa de análise temporária.

---

# Objetivo

Gerar artefatos de QA com clareza, rastreabilidade e foco em engenharia de testes:

- análise de inputs;
- identificação de gaps, riscos, edge cases e premissas;
- cenários de teste;
- automação Playwright;
- estruturas arquiteturais sustentáveis.

---

# Stack oficial

- Playwright
- JavaScript
- Node.js

---

# Linguagem padrão

- Utilizar JavaScript por padrão.
- Utilizar TypeScript ou outras linguagens suportadas pelo Playwright apenas quando explicitamente solicitado.

---

# Arquitetura padrão

Se nenhuma arquitetura for especificada, utilizar Hybrid Architecture combinando:

- Page Object Model;
- Application Actions;
- Functional Abstractions.

Referência obrigatória:

`architecture/architecture-guidelines.md`

---

# Fluxo padrão de geração

Seguir o workflow oficial:

`workflows/qa-generation-flow.md`

Resumo do fluxo:

0. Se os inputs recebidos forem sem texto após "My request for Code:", sempre perguntar ao usuário quais das opções deseja executar antes de prosseguir.
1. Analisar os inputs recebidos.
2. Identificar gaps, riscos, edge cases e premissas.
3. Definir o Test Repository do Xray quando houver geração de CSV; se o usuário não informar, gerar um nome automático coerente com o input.
4. Gerar cenários de teste rastreáveis.
5. Verificar o escopo e a autorização conforme a etapa 5 do workflow; respeitar revisões solicitadas e não repetir confirmações já dadas.
6. Gerar automação somente no escopo solicitado ou autorizado, com cenários e prontidão conforme a etapa 6 do workflow. Cenários aprovados, por si só, não autorizam implementação.
7. Respeitar a arquitetura definida ou a Hybrid Architecture por padrão.
8. Aplicar as convenções do projeto.

---

# Inputs suportados

O toolkit pode receber:

- User Stories;
- critérios de aceite;
- OpenAPI;
- Swagger;
- Figma;
- regras de negócio;
- logs;
- payloads;
- documentação técnica.

## Padrão para arquivos de input

- Preferir arquivos locais em `inputs/` quando o input for médio ou grande.
- Usar texto direto no prompt apenas para inputs pequenos ou instruções pontuais.
- Ao receber arquivos em `inputs/`, ler somente os trechos necessários para a tarefa e cruzar fontes quando houver mais de um arquivo.
- Não criar automação diretamente a partir dos arquivos de input sem antes seguir o workflow oficial.

---

# Prompts oficiais

Consultar:

- `prompts/analyze-input.md`
- `prompts/generate-scenarios.md`
- `prompts/generate-playwright-tests.md`

---

# Estrutura padrão esperada para projetos gerados

Esta estrutura deve ser usada apenas no projeto Playwright gerado, nunca diretamente dentro do `AI-QA-TOOLKIT`:

Ela representa as camadas disponíveis. Criar somente as necessárias ao projeto, conforme `architecture/architecture-guidelines.md`; projetos apenas de API não precisam de `pages/`, e `actions/` também pode compor jornadas de API.

```text
tests/
pages/
actions/
services/
helpers/
fixtures/
data/
```
