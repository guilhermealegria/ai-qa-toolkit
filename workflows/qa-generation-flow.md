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
- O conteúdo real de `inputs/` deve ser tratado como temporário e não versionado por padrão.

---

# Fluxo principal

Aplicar somente as etapas pertinentes ao pedido, reutilizando análises, cenários e decisões existentes. O fluxo permite os seguintes encaminhamentos:

| Objetivo solicitado | Referência principal | Entrega e limite |
| --- | --- | --- |
| Analisar documentação e testabilidade | [Análise de inputs](../prompts/analyze-input.md) | Diagnóstico; não iniciar implementação automaticamente. |
| Escrever ou revisar cenários funcionais | [Cenários funcionais](../prompts/generate-scenarios.md) | Cenários e exportações pertinentes ao pedido. |
| Automatizar testes funcionais de API/UI | [Playwright](../prompts/generate-playwright-tests.md) | Implementação a partir de cenários, conforme etapas 5 e 6. |
| Levantar requisitos ou escrever cenários de desempenho | [Planejamento de desempenho](../prompts/plan-non-functional-tests.md) | Requisitos mensuráveis e cenários independentes de ferramenta; sem scripts por padrão. |
| Implementar cenários de desempenho em k6 | [Geração k6](../prompts/generate-k6-tests.md) | Scripts e verificações pertinentes; geração não implica executar carga. |
| Analisar resultados de desempenho existentes | [Validação e interpretação](../prompts/plan-non-functional-tests.md#6-validação-e-interpretação) | Conclusões sustentadas pelas evidências; não iniciar nova execução automaticamente. |

Para pedidos mistos, manter rastreabilidade comum, mas usar os critérios e formatos próprios de cada objetivo. Um contrato de API pode alimentar ambos os planejamentos; sua presença não determina a ferramenta. Para outra ferramenta explicitamente escolhida, preservar a especificação de desempenho e consultar sua documentação na implementação, sem substituir a escolha por k6. Planejamento de desempenho não representa cobertura completa de segurança, acessibilidade ou outros atributos não funcionais.

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
- métricas, incidentes, objetivos de desempenho e resultados de execuções anteriores;
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

Para desempenho, usar o roteiro de [planejamento](../prompts/plan-non-functional-tests.md) para identificar jornada, carga, duração, métricas, unidades, janelas de avaliação, ambiente e evidências. Registrar valores ausentes como pendências; não usar valores de templates como requisitos reais nem exigir escolha de ferramenta para começar o levantamento.

---

## 3. Definir Test Repository do Xray

Antes de gerar arquivos para upload no Xray, identificar se o usuário informou um destino para o Test Repository.

Esta etapa se aplica somente quando a exportação estiver no escopo. Não exigir Xray nem converter fichas quantitativas de desempenho para CSV Cucumber por padrão.

Seguir a regra detalhada definida em:

`prompts/generate-scenarios.md`

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

Usar a referência de [cenários funcionais](../prompts/generate-scenarios.md).

Para desempenho, usar a matriz de requisitos e a ficha de cenário de [planejamento de desempenho](../prompts/plan-non-functional-tests.md). Selecionar Smoke, Load, Stress, Soak, Spike e Breakpoint conforme objetivo e risco, sem tornar os seis obrigatórios. Preservar modelo de carga, unidade de trabalho, perfil temporal, critérios de aceite, condições de parada e evidências necessárias. BDD pode complementar a ficha quando solicitado, sem substituir sua especificação quantitativa.

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

## 6. Gerar implementação quando autorizada e pronta

Além da autorização definida na etapa 5:

- Criar ou receber cenários e analisar os inputs relevantes antes de convertê-los em código. Uma solicitação explícita de automação dispensa nova confirmação, mas não essa preparação.
- Reutilizar análises, cenários e decisões já disponíveis; não repetir etapas concluídas sem necessidade.
- Identificar lacunas que afetem comportamento esperado, destino ou critérios de validação. Esclarecer o que bloquear a implementação e avançar nas partes independentes, sem inventar requisitos.
- Se houver revisão solicitada pelo usuário ainda pendente, aguardar sua conclusão antes de implementar.

Encaminhar conforme o objetivo e a ferramenta escolhida:

- Testes funcionais Playwright: [geração Playwright](../prompts/generate-playwright-tests.md).
- Desempenho em k6: [geração k6](../prompts/generate-k6-tests.md), preservando os requisitos quantitativos e as hipóteses do planejamento.
- Desempenho com ferramenta ainda indefinida: esclarecer a escolha antes de implementar, aproveitando o planejamento já feito.

### Execução de carga

Gerar scripts e executar carga são ações distintas. Executar carga somente quando isso estiver solicitado ou autorizado, com alvo, perfil, limites e condições de parada definidos. Aproveitar autorizações já dadas dentro desse escopo. Verificações que enviem requisições, mesmo curtas, também devem respeitar esse alvo e escopo; verificações locais de configuração não comprovam desempenho. Registrar o que foi executado e não usar endpoints públicos de exemplo como destino automático.

---

## 7. Aplicar arquitetura e convenções

Para Playwright, se o usuário não especificar arquitetura, aplicar Hybrid Architecture:

- Page Object Model;
- Application Actions;
- Functional Abstractions.

Usar como referência:

`architecture/architecture-guidelines.md`

Para k6, seguir as responsabilidades e convenções do projeto de destino descritas no [prompt k6](../prompts/generate-k6-tests.md#projeto-e-arquitetura). Usar JavaScript por padrão no runtime k6; não impor Page Objects ou Node.js como runtime dos testes. O planejamento de desempenho permanece independente de linguagem e ferramenta.

---

## 8. Entregar resultado de forma clara

Ao finalizar, informar:

- o que foi analisado;
- principais gaps e riscos;
- cenários ou arquivos gerados;
- cobertura principal;
- arquitetura usada, quando houver automação;
- próximos passos recomendados.

Em desempenho, informar também requisitos e perfis selecionados, pendências quantitativas e prontidão para implementação. Se houver execução ou análise de resultados, distinguir validade da execução, cumprimento das metas e limitações das evidências. Não declarar capacidade ou aprovação do produto com base apenas na geração dos scripts.
