# Objetivo
Gerar cenários de teste

# Entradas possíveis
Você pode receber UM dos seguintes:

## Opção A — User Story
Descrição funcional da feature + critérios de aceite

## Opção B — OpenAPI Spec
Especificação da API (YAML ou JSON)

# Instruções
- Cobrir fluxos positivos e negativos
- Incluir edge cases
- Considerar falhas externas

# Formato de saída

## 1. formato de cenários
- Por padrão utilizar BDD (Given/When/And/Then)
- Utilizar estrutura step-by-step somente se for especificado

## 2. criar arquivo com cenários
- Crie um arquivo .feature com os cenários para download caso seja bdd
- Crie um arquivo .xlsx com os cenários para download caso seja step-by-step