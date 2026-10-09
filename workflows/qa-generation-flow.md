# QA Generation Flow

Este workflow define o fluxo central do AI QA Toolkit para análise de inputs, cenários funcionais, planejamento de desempenho e implementação em Playwright ou k6 conforme o objetivo solicitado.

Ele deve ser usado por qualquer agente ou ferramenta que trabalhe neste repositório, incluindo Codex e Claude Code.

---

# Premissas

- O repositório `AI-QA-TOOLKIT` é um toolkit, não o projeto final de automação.
- A análise deve ser objetiva e útil para QAs debaterem com o time.
- A automação deve estar no escopo solicitado ou autorizado e partir de cenários claros, seguindo as condições das etapas 5 e 6.
- Projetos executáveis devem ficar fora do toolkit, conforme os prompts de implementação da ferramenta escolhida.
- Inputs médios ou grandes devem ser armazenados preferencialmente em `inputs/` para permitir leitura seletiva, buscas por trechos relevantes e menor uso de tokens.
- O conteúdo real de `inputs/` deve ser tratado como temporário e não versionado por padrão, conforme a seção "Artefatos, privacidade e versionamento".

---

# Locais e resolução de caminhos

Regras na fonte única [locais, artefatos, privacidade e versionamento](../plugins/ai-qa-toolkit/shared/artifacts-and-paths.md): três locais (recursos do toolkit, workspace de inputs e artefatos, projeto de automação). Nos arquivos de entrada na raiz (`AGENTS.md`, `CLAUDE.md`), os caminhos são relativos à raiz do toolkit.

## Artefatos, privacidade e versionamento

Destino dos artefatos, itens não versionados por padrão, exemplos fictícios, projeto consumidor e distribuição: ver [fonte única](../plugins/ai-qa-toolkit/shared/artifacts-and-paths.md#artefatos-privacidade-e-versionamento).

---

# Fluxo principal

Aplicar somente as etapas pertinentes ao pedido, reutilizando análises, cenários e decisões existentes. O fluxo permite os seguintes encaminhamentos:

| Objetivo solicitado | Referência principal | Entrega e limite |
| --- | --- | --- |
| Analisar documentação e testabilidade | [Análise de inputs](../plugins/ai-qa-toolkit/skills/qa-analysis/SKILL.md) | Diagnóstico; não iniciar implementação automaticamente. |
| Escrever ou revisar cenários funcionais | [Cenários funcionais](../plugins/ai-qa-toolkit/skills/qa-scenarios/SKILL.md) | Cenários e exportações pertinentes ao pedido. |
| Automatizar testes funcionais de API/UI | [Playwright](../plugins/ai-qa-toolkit/skills/qa-playwright/SKILL.md) | Implementação a partir de cenários, conforme etapas 5 e 6. |
| Levantar requisitos ou escrever cenários de desempenho | [Planejamento de desempenho](../plugins/ai-qa-toolkit/skills/qa-nft-plan/SKILL.md) | Requisitos mensuráveis e cenários independentes de ferramenta; sem scripts por padrão. |
| Implementar cenários de desempenho em k6 | [Geração k6](../plugins/ai-qa-toolkit/skills/qa-k6/SKILL.md) | Scripts e verificações pertinentes; geração não implica executar carga. |
| Analisar resultados de desempenho existentes | [Validação e interpretação](../plugins/ai-qa-toolkit/skills/qa-nft-plan/SKILL.md#6-validação-e-interpretação) | Conclusões sustentadas pelas evidências; não iniciar nova execução automaticamente. |

Para pedidos mistos, manter rastreabilidade comum, mas usar os critérios e formatos próprios de cada objetivo. Um contrato de API pode alimentar ambos os planejamentos; sua presença não determina a ferramenta. Para outra ferramenta explicitamente escolhida, preservar a especificação de desempenho e consultar sua documentação na implementação, sem substituir a escolha por k6. Planejamento de desempenho não representa cobertura completa de segurança, acessibilidade ou outros atributos não funcionais.

## 0. Tratar pedido sem intenção definida

Regra na skill de coordenação: [identificar a intenção](../plugins/ai-qa-toolkit/skills/qa-flow/SKILL.md#1-identificar-a-intenção). Preserva o caso de inputs sem texto após "My request for Code:", prevalece o objetivo já definido na conversa e, sem intenção identificável, pergunta qual tarefa executar.

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
- métricas, incidentes, objetivos de desempenho e resultados de execuções anteriores;
- combinação de múltiplas fontes.

Usar como referência:

[Análise de inputs](../plugins/ai-qa-toolkit/skills/qa-analysis/SKILL.md)

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

Para desempenho, usar o roteiro de [planejamento](../plugins/ai-qa-toolkit/skills/qa-nft-plan/SKILL.md) para identificar jornada, carga, duração, métricas, unidades, janelas de avaliação, ambiente e evidências. Registrar valores ausentes como pendências; não usar valores de templates como requisitos reais nem exigir escolha de ferramenta para começar o levantamento.

---

## 3. Definir Test Repository do Xray

Antes de gerar arquivos para upload no Xray, identificar se o usuário informou um destino para o Test Repository.

Esta etapa se aplica somente quando a exportação estiver no escopo. Não exigir Xray nem converter fichas quantitativas de desempenho para CSV Cucumber por padrão.

Seguir a regra detalhada definida em:

[Skill de cenários](../plugins/ai-qa-toolkit/skills/qa-scenarios/SKILL.md#definição-do-test-repository-para-xray)

---

## 4. Gerar cenários de teste rastreáveis

Para cenários funcionais, cobrir conforme aplicável:

- fluxos positivos;
- fluxos negativos;
- validações de contrato;
- regras de negócio;
- permissões e autenticação, quando aplicável;
- falhas externas, indisponibilidade, timeouts e conflitos;
- edge cases relevantes.

Usar a referência de [cenários funcionais](../plugins/ai-qa-toolkit/skills/qa-scenarios/SKILL.md).

Para desempenho, usar a matriz de requisitos e a ficha de cenário de [planejamento de desempenho](../plugins/ai-qa-toolkit/skills/qa-nft-plan/SKILL.md). Selecionar Smoke, Load, Stress, Soak, Spike e Breakpoint conforme objetivo e risco, sem tornar os seis obrigatórios. Preservar modelo de carga, unidade de trabalho, perfil temporal, critérios de aceite, condições de parada e evidências necessárias. BDD pode complementar a ficha quando solicitado, sem substituir sua especificação quantitativa.

---

## 5. Verificar o escopo e a autorização para automação

Regra na fonte única [escopo, autorização e prontidão](../plugins/ai-qa-toolkit/shared/authorization-and-scope.md#autorização-e-escopo), inclusive os exemplos de aplicação. É a referência central para a transição de cenários para implementação, inclusive nos prompts de Playwright e k6.

---

## 6. Gerar implementação quando autorizada e pronta

Além da autorização da etapa 5, aplicar a [prontidão para implementar](../plugins/ai-qa-toolkit/shared/authorization-and-scope.md#prontidão-para-implementar).

Encaminhar conforme o objetivo e a ferramenta escolhida:

- Testes funcionais Playwright: [geração Playwright](../plugins/ai-qa-toolkit/skills/qa-playwright/SKILL.md).
- Desempenho em k6: [geração k6](../plugins/ai-qa-toolkit/skills/qa-k6/SKILL.md), preservando os requisitos quantitativos e as hipóteses do planejamento.
- Desempenho com ferramenta ainda indefinida: esclarecer a escolha antes de implementar, aproveitando o planejamento já feito.

### Comandos no terminal

Regra na fonte única [comandos no terminal](../plugins/ai-qa-toolkit/shared/terminal-commands.md), válida para qualquer agente.

### Execução de carga

Regra na fonte única [execução de carga](../plugins/ai-qa-toolkit/shared/load-execution.md): gerar scripts e executar carga são ações distintas.

---

## 7. Aplicar arquitetura e convenções

Para Playwright, se o usuário não especificar arquitetura, aplicar Hybrid Architecture:

- Page Object Model;
- Application Actions;
- Functional Abstractions.

Usar como referência:

[Diretrizes de arquitetura](../plugins/ai-qa-toolkit/skills/qa-playwright/references/architecture-guidelines.md)

Para k6, seguir as responsabilidades e convenções do projeto de destino descritas no [skill k6](../plugins/ai-qa-toolkit/skills/qa-k6/SKILL.md#projeto-e-arquitetura). Usar JavaScript por padrão no runtime k6; não impor Page Objects ou Node.js como runtime dos testes. O planejamento de desempenho permanece independente de linguagem e ferramenta.

---

## 8. Entregar resultado de forma clara

Estrutura do resumo final, inclusive para desempenho: ver [entregar o resultado](../plugins/ai-qa-toolkit/skills/qa-flow/SKILL.md#4-entregar-o-resultado).
