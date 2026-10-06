# Contrato dos cenários funcionais

Fonte única do que cada cenário funcional deve conter e de como BDD e step-by-step o representam. É a saída da skill de cenários e a entrada da skill de automação Playwright. O planejamento de desempenho possui sua própria ficha quantitativa e não usa este contrato.

## Formatos

- Criar cenários em português; por padrão, usar BDD com palavras-chave em inglês (Given/When/And/Then).
- Usar step-by-step quando solicitado. Preservar o formato dos cenários recebidos para revisão ou complemento, salvo pedido de conversão.
- Cada cenário deve conter as informações abaixo, independentemente do formato.

## Informações de cada cenário

| Informação | Conteúdo esperado |
| --- | --- |
| Identificador e título | ID estável e título que descreva o comportamento; preservar IDs existentes e atribuir IDs locais quando ausentes. |
| Origem | Requisito, critério, endpoint ou trecho de documento que fundamenta o cenário. Registrar quando a fonte estiver ausente. |
| Pré-condições | Estado inicial, permissões, dependências e preparação necessária; indicar quando não se aplicarem. |
| Dados | Entradas necessárias ou referência à massa, sem expor dados sensíveis. |
| Ações | Passos ordenados da interação ou operação. |
| Resultados esperados | Comportamentos observáveis e verificáveis, ligados às ações pertinentes. |
| Pendências e premissas | Lacunas e hipóteses explícitas; indicar quando impedirem a implementação do cenário. |

## Representação

- **BDD:** representar contexto, ações e resultados nos passos; usar tags ou comentários para identificação e origem e registrar pendências junto ao cenário. Não apresentar comportamento ainda indefinido como critério confirmado.
- **Step-by-step:** usar uma linha por passo, com colunas `ID`, `Título`, `Origem`, `Pré-condições`, `Passo`, `Ação`, `Dados`, `Resultado esperado` e `Pendências/Premissas`. Manter o ID em todas as linhas do cenário e preservar a ordem dos passos. Não tratar cada linha como um cenário independente.
