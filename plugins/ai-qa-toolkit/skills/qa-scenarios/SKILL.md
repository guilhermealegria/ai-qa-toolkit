---
name: qa-scenarios
description: Gera cenários funcionais rastreáveis em BDD ou step-by-step e, quando pedido, o CSV de importação do Xray. Use para escrever ou revisar cenários de teste funcional; não gera automação.
---

# Cenários funcionais

Gere cenários de teste completos, rastreáveis e prontos para automação a partir de inputs ou de uma análise anterior. Esta skill entrega cenários e as exportações pertinentes ao pedido. Não gera automação: a passagem para implementação segue [escopo, autorização e prontidão](../../shared/authorization-and-scope.md).

Para cenários de desempenho, use `qa-nft-plan`, que tem ficha quantitativa própria.

Referências desta skill, lidas somente quando necessárias:

- [Contrato dos cenários funcionais](../../shared/scenario-contract.md): leia antes de escrever os cenários.
- [Contrato CSV Xray](references/xray-csv-contract.md) e [exemplo fictício](assets/xray-cucumber.csv): somente quando houver exportação para o Xray.
- [Escopo, autorização e prontidão](../../shared/authorization-and-scope.md)
- [Locais, artefatos, privacidade e versionamento](../../shared/artifacts-and-paths.md)

## Entradas

Um ou mais dos inputs suportados: user stories, critérios de aceite, OpenAPI ou Swagger, Figma ou descrição de UI, regras de negócio, logs, payloads, documentação técnica, mensagens de erro, exemplos de request e response, ou combinações. Podem chegar também uma análise anterior ou cenários existentes para complementar ou revisar: reutilize decisões e identificadores já disponíveis e, se faltar informação para definir um resultado esperado, registre a pendência sem inventar comportamento.

Com várias fontes, cruze as informações e destaque conflitos, gaps e premissas antes dos cenários.

Para arquivos locais, prefira `inputs/` (ou o caminho informado pelo usuário). Leia apenas os trechos necessários quando o arquivo for médio ou grande, usando buscas por critérios, endpoints, regras, campos, mensagens ou status codes antes de carregar conteúdo extenso. Use texto direto no prompt apenas para inputs pequenos ou instruções complementares.

## Instruções

- Analise a entrada e explicite gaps de especificação, riscos e premissas.
- Cubra fluxos positivos e negativos, edge cases, validações de contrato (status, schema e campos obrigatórios), regras de negócio, permissões e autenticação quando aplicável, e falhas externas (autenticação inválida, timeout, indisponibilidade, conflitos).
- Garanta rastreabilidade por endpoint ou critério em cada cenário.
- Antes de gerar CSV para Xray, defina o destino do Test Repository conforme a seção abaixo.
- Escreva os cenários conforme o [contrato dos cenários funcionais](../../shared/scenario-contract.md).

## Definição do Test Repository para Xray

Esta seção se aplica somente quando a exportação para o Xray estiver no escopo. Não exija Xray nem converta cenários step-by-step ou fichas de desempenho para CSV Cucumber por padrão.

Identifique se o usuário informou um destino no prompt ou nos metadados do pedido. Ele pode aparecer como:

```text
Repositório Xray: Nome do Repositório
Test Repository: Nome do Repositório
Test Repository Path: Pasta/Subpasta
```

- Se o usuário informar o destino, use exatamente o valor indicado na coluna `Test Repository` do CSV.
- Se não informar, gere automaticamente um nome curto, funcional e rastreável, priorizando nesta ordem: nome do produto, sistema ou API; módulo; funcionalidade principal; domínio de negócio.
- Não use dados sensíveis, nomes de pessoas, tokens, IDs internos, payloads reais ou valores temporários no nome.
- Use o mesmo destino para todos os cenários quando o input tratar uma única funcionalidade, API ou módulo. Quando cobrir módulos claramente diferentes, pode usar caminho com `/` para organizar folders e subfolders no Xray.
- Registre no resultado qual Test Repository foi usado ou gerado automaticamente.

## Artefatos e entrega

| Formato | Entrega padrão | Variações solicitadas |
| --- | --- | --- |
| BDD | Arquivo `.feature` e CSV para Xray conforme a seção seguinte. | Respeitar pedidos de somente conteúdo, de revisão ou de formatos específicos, sem gerar exportações adicionais não desejadas. |
| Step-by-step | Tabela com as colunas do contrato, apresentada na resposta. | Quando solicitado arquivo de planilha, gerar um `.xlsx` real com a mesma estrutura. Não aplicar o CSV Cucumber a cenários step-by-step. |

- Não adicione os artefatos de cenários ao projeto final de automação. A automação pode consumi-los sem copiar os arquivos para o projeto.
- Siga as regras de destino e versionamento de [locais e artefatos](../../shared/artifacts-and-paths.md).
- Informe o caminho ou link de cada arquivo efetivamente criado. Conteúdo na resposta deve ser identificado como conteúdo, não como arquivo gerado.
- Uma tabela Markdown ou um CSV renomeado não constitui um arquivo `.xlsx`.
- Se a criação de um arquivo solicitado estiver indisponível, informe a limitação e apresente o conteúdo disponível, deixando explícito qual artefato permanece pendente.

## CSV para Xray

Quando a exportação BDD se aplicar, siga o [contrato CSV](references/xray-csv-contract.md) e o [exemplo fictício](assets/xray-cucumber.csv).

- Preserve as dez colunas do contrato, um registro lógico por cenário e os mesmos IDs usados nos demais artefatos.
- Use o destino definido na seção do Test Repository.
- Confira a serialização e o conteúdo conforme o contrato antes de entregar.
- Informe o perfil usado e as pendências de configuração do importador. O perfil documentado usa Xray Cloud; se a edição não for informada, entregue com essa premissa explícita. Para outra edição ou versão, confira o mapeamento aplicável sem prometer compatibilidade automática.
- Distinga validação local de importação real. Gerar CSV não autoriza enviá-lo ao Xray.

## Saída obrigatória

Apresente nesta ordem:

1. Resumo do input analisado
2. Gaps, riscos e edge cases identificados
3. Cenários de teste
4. Artefatos conforme o formato e o pedido: caminhos dos arquivos criados ou conteúdo apresentado, distinguindo entregas concluídas e pendentes
5. Test Repository usado no Xray e CSV produzido, somente quando essa exportação se aplicar
6. Pendências que afetem a implementação dos cenários
7. Próximo passo conforme [escopo e autorização](../../shared/authorization-and-scope.md): encerrar quando o pedido se limitar a cenários, aguardar revisão solicitada ou continuar a automação já autorizada. Pergunte sobre a transição somente quando houver intenção de continuar e a próxima etapa estiver indefinida.
