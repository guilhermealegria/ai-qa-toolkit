# AI QA Toolkit - Instruções para Claude Code

Este repositório é um toolkit para apoiar análise de requisitos, geração de cenários e automação Playwright com IA.

Ele NÃO é o projeto final de automação.

---

# Regras obrigatórias

- Não criar projetos Playwright dentro deste repositório.
- Não criar as pastas `tests/`, `pages/`, `actions/`, `services/`, `fixtures/`, `helpers/` ou `data/` diretamente dentro do `AI-QA-TOOLKIT`, exceto quando a tarefa for alterar exemplos, documentação ou templates do próprio toolkit.
- Quando for necessário gerar automação, criar o projeto Playwright fora deste repositório.
- Antes de gerar automação, criar ou receber cenários de teste e aplicar as condições de escopo, autorização e prontidão das etapas 5 e 6 de `workflows/qa-generation-flow.md`.
- Não assumir informações ausentes nos inputs; registrar gaps, riscos, edge cases e premissas.
- Preferir arquivos de entrada em `inputs/` quando o conteúdo for médio ou grande, para permitir leitura seletiva e reduzir uso de tokens.
- Não versionar o conteúdo real de `inputs/` por padrão, pois pode conter documentação interna, dados sensíveis ou massa temporária de análise.

---

# Execução de comandos

- Antes de executar qualquer comando no terminal, identificar o shell ativo informado pelo ambiente, pela IDE ou pelo contexto da sessão.
- Adaptar a sintaxe dos comandos ao shell ativo. Se o terminal for PowerShell, usar comandos e sintaxe de PowerShell.
- Não assumir Bash, Linux ou WSL por padrão.
- Evitar comandos com sintaxe exclusiva de Bash, como `&&`, `||`, `export`, `source`, `grep`, `sed`, `awk`, redirecionamentos ou expansões específicas, quando o shell ativo for PowerShell.
- Quando houver dúvida sobre o shell ativo, verificar primeiro com um comando compatível ou perguntar ao usuário antes de executar comandos dependentes do shell.
- Em ambiente Windows com PowerShell, preferir cmdlets como `Get-ChildItem`, `Get-Content`, `Select-String`, `Set-Location`, `$env:VAR = "valor"` e `Remove-Item` com cuidado explícito.

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
3. Definir o Test Repository do Xray quando houver geração de CSV; se o usuário não informar, gerar um nome automático coerente com o input.
4. Gerar cenários de teste rastreáveis.
5. Verificar o escopo e a autorização conforme a etapa 5 do workflow; respeitar revisões solicitadas e não repetir confirmações já dadas.
6. Gerar automação somente no escopo solicitado ou autorizado, com cenários e prontidão conforme a etapa 6 do workflow. Cenários aprovados, por si só, não autorizam implementação.
7. Aplicar a arquitetura definida ou a arquitetura padrão.
8. Entregar resumo claro do que foi gerado.

---

# Prompts oficiais

Usar os prompts abaixo como fonte principal:

- `prompts/analyze-input.md`
- `prompts/generate-scenarios.md`
- `prompts/generate-playwright-tests.md`

---

# Inputs

Quando o usuário fornecer documentação, contratos, payloads, logs, regras de negócio ou outros materiais suportados, usar preferencialmente:

```text
inputs/
```

Orientações:

- Ler arquivos locais em `inputs/` somente conforme a necessidade da tarefa.
- Evitar carregar arquivos inteiros no contexto quando for possível localizar trechos relevantes.
- Usar texto colado no prompt apenas para inputs pequenos ou instruções complementares.

---

# Conduta esperada

- Ser objetivo e útil para QAs debaterem com o time.
- Separar fatos observados, gaps, riscos, premissas e perguntas.
- Manter rastreabilidade entre requisitos, cenários e automação.
- Evitar overengineering.
- Respeitar as convenções já existentes no toolkit.
