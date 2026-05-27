# Objetivo
Gerar cenários de teste completos, rastreáveis e prontos para automação.

# Entradas possíveis
Você pode receber um ou mais dos seguintes inputs suportados pelo toolkit:

- User Story;
- critérios de aceite;
- OpenAPI ou Swagger;
- Figma ou descrição de UI;
- regras de negócio;
- logs;
- payloads;
- documentação técnica;
- mensagens de erro;
- exemplos de request e response;
- combinação de múltiplas fontes.

Quando houver múltiplas fontes, cruzar as informações e destacar conflitos, gaps e premissas antes dos cenários.

## Origem dos inputs

Os inputs podem chegar como texto no prompt, arquivo anexado ou arquivo local.

Quando houver arquivos locais, preferir o diretório:

```text
inputs/
```

Para reduzir uso de tokens:

- Ler somente os trechos necessários quando o arquivo for médio ou grande.
- Usar buscas por critérios, endpoints, regras, campos, mensagens ou status codes antes de carregar conteúdo extenso.
- Usar texto direto no prompt apenas para inputs pequenos ou instruções complementares.

# Instruções
- Analisar a entrada e explicitar gaps de especificação, riscos e premissas.
- Cobrir fluxos positivos e negativos.
- Incluir edge cases e validações de contrato (status, schema e campos obrigatórios).
- Considerar falhas externas (ex.: auth inválida, timeout, indisponibilidade e conflitos).
- Garantir rastreabilidade por endpoint/critério em cada cenário.

# Formato de saída

## 1. formato de cenários
- Por padrão utilizar BDD (Given/When/And/Then)
- Utilizar estrutura step-by-step somente se for especificado

## 2. criar arquivo com cenários
- Se for BDD, gerar conteúdo em `.feature`.
- Se for BDD, gerar também um arquivo `.csv` para upload dos cenários no Xray.
- Se for step-by-step, gerar conteúdo tabular pronto para `.xlsx`.
- Não adicionar o arquivo no projeto final de automação.

## 3. arquivo CSV para Xray

Quando gerar cenários em BDD, criar também um CSV compatível com importação no Xray usando como referência:

`examples/Importação_TC Cucumber_Exemplo(in).csv`

Regras para o CSV:

- Usar separador `;`.
- Manter o cabeçalho no mesmo formato do exemplo:

```csv
TestID;Test type;Summary;Description;Priority;Action;Data;Expected Result;Gherkin definition;Test Repository,,
```

- Criar uma linha por cenário.
- Preencher `Test type` com `Cucumber`.
- Preencher `Summary` com um título curto e rastreável do cenário.
- Preencher `Description` com uma descrição objetiva do cenário.
- Preencher `Priority` com `High`, `Medium` ou `Low`, conforme risco e criticidade.
- Deixar `Action`, `Data` e `Expected Result` vazios para cenários Cucumber, mantendo os separadores.
- Preencher `Gherkin definition` com o cenário em Given/When/And/Then.
- Preencher `Test Repository` quando houver contexto de projeto, módulo ou funcionalidade; caso contrário, usar um nome funcional coerente com o input.
- Não incluir dados sensíveis no CSV.

## 4. saída obrigatória
Apresentar nesta ordem:
1. Resumo do input analisado
2. Gaps/riscos/edge cases identificados
3. Cenários de teste
4. Arquivo `.feature` gerado ou conteúdo sugerido
5. Arquivo `.csv` para upload no Xray, quando aplicável
6. Pergunta de confirmação: "Você quer revisar os cenários antes da automação?"
