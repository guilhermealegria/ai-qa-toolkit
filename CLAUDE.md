# AI QA Toolkit - Instruções para Claude Code

Este repositório é um toolkit para análise de requisitos, cenários funcionais, automação Playwright, planejamento de desempenho e geração de scripts k6 com IA.

Ele NÃO é o projeto final de automação.

---

# Regras obrigatórias

- Não criar projetos executáveis Playwright ou k6 dentro deste repositório.
- Não criar as pastas `tests/`, `pages/`, `actions/`, `services/`, `fixtures/`, `helpers/` ou `data/` diretamente dentro do `AI-QA-TOOLKIT`, exceto quando a tarefa for alterar exemplos, documentação ou templates do próprio toolkit.
- Quando for necessário gerar automação, criar ou atualizar o projeto executável fora deste repositório.
- Antes de gerar automação, criar ou receber cenários de teste e aplicar as condições de escopo, autorização e prontidão das etapas 5 e 6 de `workflows/qa-generation-flow.md`.
- Não assumir informações ausentes nos inputs; registrar gaps, riscos, edge cases e premissas.
- Preferir arquivos de entrada em `inputs/` quando o conteúdo for médio ou grande, para permitir leitura seletiva e reduzir uso de tokens.
- Não versionar o conteúdo real de `inputs/` por padrão, pois pode conter documentação interna, dados sensíveis ou massa temporária de análise.

---

# Execução de comandos

- Antes de executar qualquer comando no terminal, identificar o shell ativo e adaptar a sintaxe a ele; não assumir Bash, Linux ou WSL por padrão.
- Regra completa, incluindo PowerShell: seção "Comandos no terminal" de `workflows/qa-generation-flow.md`.

---

# Stack padrão

- Playwright
- JavaScript
- Node.js

Use JavaScript por padrão. Use TypeScript ou outra linguagem suportada pelo Playwright somente quando o usuário solicitar explicitamente.

Essa stack se aplica à automação Playwright. O planejamento de desempenho é independente de ferramenta; a implementação especializada em k6 usa JavaScript e o runtime k6. Gerar scripts não implica executar carga; seguir o escopo de execução definido no workflow.

---

# Arquitetura padrão

Para Playwright, quando o usuário não especificar arquitetura, usar Hybrid Architecture combinando:

- Page Object Model;
- Application Actions;
- Functional Abstractions.

Referência:

`architecture/architecture-guidelines.md`

Para k6, seguir `prompts/generate-k6-tests.md` e as convenções do projeto de destino, sem impor a arquitetura Playwright.

---

# Workflow obrigatório

Seguir o fluxo central:

`workflows/qa-generation-flow.md`

Resumo:

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

# Prompts oficiais

Usar os prompts abaixo como fonte principal:

- `skills/qa-analysis/SKILL.md` — análise de inputs
- `prompts/generate-scenarios.md`
- `prompts/generate-playwright-tests.md`
- `prompts/plan-non-functional-tests.md` — planejamento de desempenho independente de ferramenta.
- `prompts/generate-k6-tests.md` — implementação dos cenários de desempenho em k6.

---

# Inputs

Os tipos de input aceitos e a forma de lê-los estão na etapa 1 de `workflows/qa-generation-flow.md`; a técnica de leitura seletiva está em `skills/qa-analysis/SKILL.md`.

- Preferir arquivos locais em `inputs/` quando o conteúdo for médio ou grande; usar texto colado no prompt apenas para inputs pequenos ou instruções complementares.

---

# Conduta esperada

- Ser objetivo e útil para QAs debaterem com o time.
- Separar fatos observados, gaps, riscos, premissas e perguntas.
- Manter rastreabilidade entre requisitos, cenários e automação.
- Evitar overengineering.
- Respeitar as convenções já existentes no toolkit.
