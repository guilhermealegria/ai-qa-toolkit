# Locais, artefatos, privacidade e versionamento

Fonte única das regras de onde ler recursos, onde gravar artefatos e o que não versionar.

## Três locais

Distinguir os locais, sem confundir o que o toolkit distribui com o que o usuário fornece ou recebe:

| Local | Conteúdo | Regra |
| --- | --- | --- |
| Recursos do toolkit | Skills, referências e exemplos fictícios distribuídos com o toolkit. | Somente leitura durante o uso. Resolver cada referência a partir do documento que a declara. Não exigir que o usuário os copie para o projeto dele. |
| Workspace de inputs e artefatos | Inputs do usuário (preferencialmente `inputs/`) e artefatos de QA gerados, como cenários e exportações. | Respeitar o caminho informado pelo usuário, sem exigir cópia para dentro do toolkit. |
| Projeto de automação | Código Playwright ou k6 e seus arquivos de configuração. | Sempre fora do toolkit e de sua instalação, conforme a skill da ferramenta escolhida. |

Procurar recursos do toolkit apenas nos próprios recursos, nunca no projeto de automação do usuário; e gravar artefatos apenas no workspace ou no projeto de destino, nunca nos recursos do toolkit.

## Artefatos, privacidade e versionamento

- **Destino dos artefatos de QA** (cenários, fichas de desempenho, exportações): usar o destino informado pelo usuário ou uma convenção já definida no workspace. Quando o workspace for o checkout do toolkit, a convenção é `generated-scenarios/`, já excluída do Git. Sem destino inequívoco, esclarecê-lo antes de gravar e avançar no conteúdo independente dessa decisão.
- **Não versionar por padrão:** o conteúdo real de `inputs/`, os artefatos gerados e as saídas de execução, pois podem conter documentação interna, dados sensíveis ou massa temporária. As exceções são os arquivos explicitamente liberados, como o exemplo fictício `examples/xray-cucumber.csv`, o `inputs/README.md` e os `.gitkeep`.
- **Exemplos e referências distribuídos** devem ser fictícios, sem credenciais, chaves, destinos reais ou dados de clientes.
- **Projeto consumidor:** não alterar automaticamente o versionamento nem os arquivos de exclusão de um projeto de automação existente; sugerir a alteração quando for relevante.
- **Distribuição:** o conteúdo de um pacote deve ser definido por lista explícita de arquivos, sem depender apenas das exclusões do Git. Essa definição pertence à etapa de empacotamento.
