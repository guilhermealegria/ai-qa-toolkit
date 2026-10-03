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

Também podem ser recebidos uma análise anterior ou cenários existentes para complementar ou revisar. Reutilizar decisões e identificadores já disponíveis. Se faltarem informações para definir um resultado esperado, registrar a pendência sem inventar comportamento.

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
- Antes de gerar CSV para Xray, definir o destino do Test Repository conforme a seção "Definição do Test Repository para Xray".

## Definição do Test Repository para Xray

Identificar se o usuário informou um destino para o Test Repository no prompt ou nos metadados do pedido.

O destino pode aparecer como:

```text
Repositório Xray: Nome do Repositório
Test Repository: Nome do Repositório
Test Repository Path: Pasta/Subpasta
```

Regras:

- Se o usuário informar o destino, usar exatamente o valor indicado na coluna `Test Repository` do CSV.
- Se o usuário não informar o destino, gerar automaticamente um nome curto, funcional e rastreável.
- O nome automático deve priorizar, nesta ordem:
  1. nome do produto, sistema ou API;
  2. módulo;
  3. funcionalidade principal;
  4. domínio de negócio.
- Não usar dados sensíveis, nomes de pessoas, tokens, IDs internos, payloads reais ou valores temporários no nome.
- Usar o mesmo destino para todos os cenários quando o input tratar uma única funcionalidade, API ou módulo.
- Quando o input cobrir módulos claramente diferentes, pode ser usado caminho com `/` para organizar folders/subfolders no Xray.
- Registrar no resultado qual Test Repository foi usado ou gerado automaticamente.

# Formato de saída

## 1. Contrato dos cenários funcionais

- Criar cenários em português; por padrão, usar BDD com palavras-chave em inglês (Given/When/And/Then).
- Usar step-by-step quando solicitado. Preservar o formato dos cenários recebidos para revisão ou complemento, salvo pedido de conversão.
- Cada cenário deve conter as informações abaixo, independentemente do formato. Este contrato se aplica a cenários funcionais; o planejamento de desempenho possui sua própria ficha quantitativa.

| Informação | Conteúdo esperado |
| --- | --- |
| Identificador e título | ID estável e título que descreva o comportamento; preservar IDs existentes e atribuir IDs locais quando ausentes. |
| Origem | Requisito, critério, endpoint ou trecho de documento que fundamenta o cenário. Registrar quando a fonte estiver ausente. |
| Pré-condições | Estado inicial, permissões, dependências e preparação necessária; indicar quando não se aplicarem. |
| Dados | Entradas necessárias ou referência à massa, sem expor dados sensíveis. |
| Ações | Passos ordenados da interação ou operação. |
| Resultados esperados | Comportamentos observáveis e verificáveis, ligados às ações pertinentes. |
| Pendências e premissas | Lacunas e hipóteses explícitas; indicar quando impedirem a implementação do cenário. |

Em BDD, representar contexto, ações e resultados nos passos; usar tags ou comentários para identificação e origem e registrar pendências junto ao cenário. Não apresentar comportamento ainda indefinido como critério confirmado.

Em step-by-step, usar uma linha por passo, com colunas `ID`, `Título`, `Origem`, `Pré-condições`, `Passo`, `Ação`, `Dados`, `Resultado esperado` e `Pendências/Premissas`. Manter o ID em todas as linhas do cenário e preservar a ordem dos passos. Não tratar cada linha como um cenário independente.

## 2. Artefatos e entrega

| Formato | Entrega padrão | Variações solicitadas |
| --- | --- | --- |
| BDD | Arquivo `.feature` e CSV para Xray conforme a seção 3. | Respeitar pedidos de somente conteúdo, de revisão ou de formatos específicos, sem gerar exportações adicionais não desejadas. |
| Step-by-step | Tabela com as colunas da seção 1, apresentada na resposta. | Quando solicitado arquivo de planilha, gerar um `.xlsx` real com a mesma estrutura. Não aplicar o CSV Cucumber a cenários step-by-step. |

- Não adicionar os artefatos de cenários ao projeto final de automação. A automação pode consumir esses cenários sem copiar os arquivos para o projeto.
- Usar o destino de artefatos informado ou uma convenção já definida no workspace; se não houver destino inequívoco, esclarecê-lo antes de gravar os arquivos e avançar no conteúdo independente dessa decisão.
- Informar o caminho ou link de cada arquivo efetivamente criado. Conteúdo na resposta deve ser identificado como conteúdo, não como arquivo gerado.
- Uma tabela Markdown ou um CSV renomeado não constitui um arquivo `.xlsx`.
- Se a criação de um arquivo solicitado estiver indisponível, informar a limitação e apresentar o conteúdo disponível, deixando explícito qual artefato permanece pendente.

## 3. CSV para Xray

Quando a exportação BDD se aplicar, seguir o [contrato CSV do toolkit](../references/xray-csv-contract.md) e o [exemplo fictício versionado](../examples/xray-cucumber.csv).

- Preservar as dez colunas do contrato, um registro lógico por cenário e os mesmos IDs usados nos demais artefatos.
- Usar o destino definido na seção "Definição do Test Repository para Xray".
- Conferir a serialização e o conteúdo conforme a referência antes de entregar.
- Informar o perfil usado e as pendências de configuração do importador. O perfil documentado usa Xray Cloud; se a edição não for informada, entregar com essa premissa explícita. Para outra edição ou versão, conferir o mapeamento aplicável sem prometer compatibilidade automática.
- Distinguir validação local de importação real. Gerar CSV não autoriza enviá-lo ao Xray.

## 4. saída obrigatória
Apresentar nesta ordem:
1. Resumo do input analisado
2. Gaps/riscos/edge cases identificados
3. Cenários de teste
4. Artefatos conforme o formato e o pedido: caminhos dos arquivos criados ou conteúdo apresentado, distinguindo entregas concluídas e pendentes
5. Test Repository usado no Xray e CSV produzido, somente quando essa exportação se aplicar
6. Pendências que afetem a implementação dos cenários
7. Próximo passo conforme as [condições de escopo e autorização do workflow](../workflows/qa-generation-flow.md#5-verificar-o-escopo-e-a-autorização-para-automação): encerrar quando o pedido se limitar a cenários, aguardar revisão solicitada ou continuar a automação já autorizada. Perguntar sobre a transição somente quando houver intenção de continuar e a próxima etapa estiver indefinida.
