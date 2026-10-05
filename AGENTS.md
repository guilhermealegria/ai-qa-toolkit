# Projeto

AI QA Toolkit para análise de requisitos, cenários funcionais, automação Playwright, planejamento de desempenho e geração de scripts k6 com apoio de IA.

Este repositório é um toolkit de apoio para QA. Ele NÃO é o projeto final de automação.

---

# Regras obrigatórias do repositório

- Não criar projetos executáveis Playwright ou k6 diretamente dentro deste repositório.
- Não criar as pastas `tests/`, `pages/`, `actions/`, `services/`, `fixtures/`, `helpers/` ou `data/` dentro deste repositório, exceto quando a tarefa for alterar exemplos, documentação ou templates do próprio toolkit.
- Projetos de automação gerados devem ser criados fora da pasta `AI-QA-TOOLKIT`.
- Usar este repositório como fonte de prompts, workflows, padrões arquiteturais e convenções.
- Arquivos de entrada recebidos do usuário devem ser colocados preferencialmente em `inputs/` para reduzir uso de tokens, permitindo leitura localizada e busca por trechos relevantes.
- O conteúdo real de `inputs/` não deve ser versionado por padrão, pois pode conter documentação interna, dados sensíveis ou massa de análise temporária.
- Antes de executar qualquer comando no terminal, identificar o shell ativo e adaptar a sintaxe a ele; não assumir Bash, Linux ou WSL por padrão. Regra completa na seção "Comandos no terminal" de `workflows/qa-generation-flow.md`.

---

# Objetivo

Gerar artefatos de QA com clareza, rastreabilidade e foco em engenharia de testes:

- análise de inputs;
- identificação de gaps, riscos, edge cases e premissas;
- cenários de teste;
- automação Playwright;
- planejamento de desempenho e implementação em k6;
- estruturas arquiteturais sustentáveis.

---

# Stack oficial

- Playwright
- JavaScript
- Node.js

Essa stack se aplica à automação Playwright. Para desempenho, o planejamento é independente de ferramenta; a implementação especializada em k6 usa JavaScript e o runtime k6. Gerar scripts não implica executar carga; seguir o escopo de execução definido no workflow.

---

# Linguagem padrão

- Utilizar JavaScript por padrão.
- Utilizar TypeScript ou outras linguagens suportadas pelo Playwright apenas quando explicitamente solicitado.

---

# Arquitetura padrão

Para Playwright, se nenhuma arquitetura for especificada, utilizar Hybrid Architecture combinando:

- Page Object Model;
- Application Actions;
- Functional Abstractions.

Referência obrigatória:

`architecture/architecture-guidelines.md`

Para k6, seguir `prompts/generate-k6-tests.md` e as convenções do projeto de destino, sem impor a arquitetura Playwright.

---

# Fluxo padrão de geração

Seguir o workflow oficial:

`workflows/qa-generation-flow.md`

Resumo do fluxo:

0. Verificar a intenção conforme a etapa 0 do workflow: aproveitar o objetivo já definido na conversa, inclusive quando "My request for Code:" estiver vazio. Perguntar qual tarefa executar somente quando não houver intenção identificável na mensagem nem no contexto.
1. Identificar o objetivo solicitado e analisar os inputs usando o encaminhamento do workflow.
2. Identificar gaps, riscos, edge cases e premissas.
3. Definir o Test Repository do Xray quando houver geração de CSV; se o usuário não informar, gerar um nome automático coerente com o input.
4. Gerar cenários rastreáveis no formato funcional ou na ficha quantitativa de desempenho, conforme o pedido.
5. Verificar o escopo e a autorização conforme a etapa 5 do workflow; respeitar revisões solicitadas e não repetir confirmações já dadas.
6. Gerar automação somente no escopo solicitado ou autorizado, com cenários e prontidão conforme a etapa 6 do workflow. Cenários aprovados, por si só, não autorizam implementação.
7. Aplicar as convenções da ferramenta escolhida; Hybrid Architecture é o padrão somente para Playwright.
8. Entregar resumo claro do que foi gerado.

---

# Inputs suportados

Os tipos de input aceitos e a forma de lê-los estão na etapa 1 de `workflows/qa-generation-flow.md`; a técnica de leitura seletiva está em `skills/qa-analysis/SKILL.md`.

- Preferir arquivos locais em `inputs/` quando o input for médio ou grande; usar texto direto no prompt apenas para inputs pequenos ou instruções pontuais.
- Não criar automação diretamente a partir dos arquivos de input sem antes seguir o workflow oficial.

---

# Prompts oficiais

Consultar:

- `skills/qa-analysis/SKILL.md` — análise de inputs
- `prompts/generate-scenarios.md`
- `prompts/generate-playwright-tests.md`
- `prompts/plan-non-functional-tests.md` — planejamento de desempenho independente de ferramenta.
- `prompts/generate-k6-tests.md` — implementação dos cenários de desempenho em k6.

---

# Estrutura padrão esperada para projetos gerados

As camadas disponíveis (`tests/`, `pages/`, `actions/`, `services/`, `helpers/`, `fixtures/`, `data/`) e os critérios para criá-las estão em `architecture/architecture-guidelines.md`. Aplicam-se apenas ao projeto Playwright gerado, nunca diretamente dentro do `AI-QA-TOOLKIT`.
