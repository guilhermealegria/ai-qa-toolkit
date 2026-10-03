# Contrato CSV para cenários Cucumber

## Escopo

Convenção do AI QA Toolkit para preparar criação de testes Cucumber pelo Test Case Importer. Base documental: Xray Cloud. A geração não envia dados ao Jira/Xray. Edição, configuração e importação real devem ser informadas separadamente da validação local.

O importador permite mapear nomes de colunas para campos. O identificador de agrupamento, Summary e Test Type são obrigatórios; campos adicionais dependem do projeto. Atualizações usam Issue Key. O projeto é selecionado no assistente quando não consta no arquivo. [Documentação oficial do importador](https://getxraydocs.atlassian.net/wiki/spaces/XRAYCLOUD/pages/44565062)

## Colunas do toolkit

Manter esta ordem e grafia:

```csv
TestID;Test type;Summary;Description;Priority;Action;Data;Expected Result;Gherkin definition;Test Repository
```

| Coluna | Regra do toolkit / mapeamento |
| --- | --- |
| TestID | ID local estável do cenário, não vazio e único no arquivo; mapear para o identificador de agrupamento, chamado Issue Id ou Test ID na documentação. Não mapear para Issue Key. |
| Test type | `Cucumber`; mapear para Test Type. |
| Summary | Título curto com ID do cenário; mapear para Summary. |
| Description | Objetivo, origem e premissas pertinentes; mapear para Description. Não inventar chave Jira para representar rastreabilidade. |
| Priority | `High`, `Medium` ou `Low` conforme risco; mapear para Priority e ajustar aos valores do projeto quando conhecidos. |
| Action | Vazio neste perfil Cucumber; deixar sem mapeamento. |
| Data | Vazio neste perfil Cucumber; deixar sem mapeamento. Dados do cenário ficam no Gherkin. |
| Expected Result | Vazio neste perfil Cucumber; deixar sem mapeamento. As expectativas ficam no Gherkin. |
| Gherkin definition | Passos do cenário com quebras reais de linha; mapear para a definição Gherkin do teste. |
| Test Repository | Destino informado ou gerado conforme o prompt de cenários; mapear para o campo de pasta do Test Repository disponível no importador. Não é a chave do projeto Jira. |

O cabeçalho é uma convenção do toolkit, não um esquema universal de toda instalação Xray. Não incluir colunas extras silenciosamente. Se o projeto exigir campos adicionais, documentar a extensão e o mapeamento específico.

## Identidade e conteúdo

- Preservar IDs existentes. Quando ausentes, atribuir `CT-001`, `CT-002` etc., sem colisões com os já recebidos, e usar os mesmos IDs no `.feature`, no CSV e na rastreabilidade da automação.
- Se IDs recebidos colidirem, registrar o conflito e resolvê-lo antes de concluir a exportação; não combinar cenários diferentes.
- Este perfil não contém Issue Key e não define atualização de issues existentes. Reimportar não deve ser tratado como operação idempotente baseada em TestID. Uma solicitação de atualização exige contrato próprio com chaves reais e autorização correspondente.
- Gerar um registro lógico por cenário. Um campo multilinha ocupa várias linhas físicas sem representar vários registros.
- Neste perfil, exportar os passos Given/When/And/Then de um cenário concreto. O título fica em Summary; não copiar o arquivo `.feature` completo, sua linha Feature ou suas tags para cada célula.
- Incluir o contexto necessário de um Background nos passos de cada teste exportado, preservando a semântica.
- Para Scenario Outline, adotar neste perfil expansão em cenários concretos, um por conjunto de dados, com IDs derivados rastreáveis (por exemplo `CT-010-01`). Manter a correspondência nos artefatos. Se o usuário exigir preservar o Outline, conferir o formato aceito pelo importador e documentar a variação antes de exportar.
- Registrar cenários cujo resultado esperado esteja indefinido como pendentes; não fabricar um Then para torná-los importáveis.
- Usar apenas dados apropriados à distribuição. O exemplo incluído é inteiramente fictício e não representa requisitos reais.

Os exemplos oficiais incluem definição Cucumber composta de passos e destacam que prioridades devem corresponder ao projeto. [Exemplos oficiais do Test Case Importer](https://getxraydocs.atlassian.net/wiki/spaces/XRAYCLOUD/pages/44565495)

## Serialização

- Usar UTF-8 sem BOM e delimitador `;` como padrão do toolkit; selecionar a mesma codificação e delimitador no importador.
- Usar aspas duplas para envolver campos com `;`, aspas ou quebras de linha. Duplicar aspas internas: o texto `item "A"` vira `"item ""A"""` na representação CSV.
- Usar quebras reais de linha nos passos, não os caracteres literais `\n`.
- Preservar campos vazios e dez colunas em cada registro; não adicionar delimitador após a última coluna nem uma linha `sep=;` antes do cabeçalho.
- Usar um serializador CSV ou conferir a saída com um parser; não contar registros por linhas nem separar campos com `split(';')`.

O importador documenta seleção de encoding/delimitador, campos entre aspas para delimitadores internos ou quebras de linha e uma opção para criação de pastas ausentes. [Configuração e preparação do CSV](https://getxraydocs.atlassian.net/wiki/spaces/XRAYCLOUD/pages/44565062)

## Conferência e entrega

1. Ler o arquivo final com parser CSV e conferir cabeçalho, dez campos, quantidade de cenários e IDs únicos.
2. Conferir campos obrigatórios preenchidos, tipo Cucumber, campos manuais vazios e prioridade coerente.
3. Comparar texto desserializado com os cenários: acentos, aspas, ponto e vírgula, dados, contexto e ordem dos passos devem permanecer intactos.
4. Conferir o Test Repository e a correspondência com os IDs do `.feature`.
5. Informar que a importação requer seleção de projeto, mapeamento de campos, prioridades e configuração de pastas apropriados. Se edição ou campos obrigatórios forem desconhecidos, registrar essa pendência sem impedir a preparação do conteúdo.
6. Relatar separadamente: CSV gerado, verificações locais realizadas e importação real realizada ou não. Não alegar importação bem-sucedida apenas porque o parser leu o arquivo.

Exemplo: [xray-cucumber.csv](../examples/xray-cucumber.csv). Contém dois cenários fictícios de catálogo, com acentos, aspas, ponto e vírgula e Gherkin multilinha. Não inclui credenciais, chaves Jira ou destinos reais.
