# Objetivo
Gerar cenários de teste completos, rastreáveis e prontos para automação.

# Entradas possíveis
Você pode receber UM dos seguintes:

## Opção A — User Story
Descrição funcional da feature + critérios de aceite

## Opção B — OpenAPI Spec
Especificação da API (YAML ou JSON)

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
- Se for step-by-step, gerar conteúdo tabular pronto para `.xlsx`.
- Não adicionar o arquivo no projeto final de automação.

## 3. saída obrigatória
Apresentar nesta ordem:
1. Resumo do input analisado
2. Gaps/riscos/edge cases identificados
3. Cenários de teste
4. Pergunta de confirmação: "Você quer revisar os cenários antes da automação?"
