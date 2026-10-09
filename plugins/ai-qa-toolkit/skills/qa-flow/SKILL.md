---
name: qa-flow
description: Ponto de entrada dos pedidos de QA. Use quando o usuário enviar apenas arquivos, links ou trechos de requisitos sem dizer o que fazer, quando a mensagem vier vazia após "My request for Code:", quando perguntar "o que fazer com isto" ou "o que acha?" sobre material de QA, ou quando pedir mais de uma etapa (análise, cenários, Playwright, desempenho, k6). Ative esta skill antes de abrir ou ler os arquivos recebidos. Identifica o objetivo, pergunta o que falta e encaminha para a skill certa, aplicando escopo e autorização.
---

# Coordenação do fluxo de QA

Esta skill decide qual capacidade usar e até onde avançar. Não analisa, não escreve cenários e não gera código: encaminha para a skill própria de cada etapa e respeita o escopo pedido.

Se o pedido já indicar uma única capacidade com clareza, use direto a skill dela, sem passar por aqui.

Referências desta skill, lidas somente quando necessárias:

- [Escopo, autorização e prontidão](../../shared/authorization-and-scope.md)
- [Execução de carga](../../shared/load-execution.md)
- [Locais, artefatos, privacidade e versionamento](../../shared/artifacts-and-paths.md)

## 1. Identificar a intenção

Primeiro verificar se o objetivo está definido na mensagem ou no contexto da conversa. Se já estiver, prosseguir nesse escopo sem perguntar de novo, inclusive quando não houver texto após "My request for Code:". Novos arquivos enviados depois desse pedido são inputs dele.

Quando não houver intenção identificável na mensagem nem no contexto, perguntar qual tarefa executar, oferecendo as opções da tabela da seção 2. Isso inclui mensagens com apenas arquivos, trechos ou links, em qualquer interface, e perguntas vagas como "o que acha?" ou "o que fazer com isto?" sobre material de QA. Faça a pergunta em português, com as opções, antes de qualquer outra ação.

- Se a intenção estiver parcialmente definida (por exemplo, o tipo de entrega ou a ferramenta de desempenho), perguntar somente o que falta.
- Enquanto a intenção não estiver clara, não avançar para análise, cenários ou automação, nem inferir o objetivo apenas pelo tipo do arquivo. É permitido identificar o tipo dos inputs para oferecer opções adequadas, sem ler conteúdo além do necessário.

## 2. Encaminhar

Aplicar somente as etapas pertinentes ao pedido, reutilizando análises, cenários e decisões existentes.

| Objetivo solicitado | Skill | Entrega e limite |
| --- | --- | --- |
| Analisar documentação e testabilidade | `qa-analysis` | Diagnóstico; não iniciar implementação automaticamente. |
| Escrever ou revisar cenários funcionais | `qa-scenarios` | Cenários e exportações pertinentes ao pedido. |
| Automatizar testes funcionais de API/UI | `qa-playwright` | Implementação a partir de cenários, conforme escopo e autorização. |
| Levantar requisitos ou escrever cenários de desempenho | `qa-nft-plan` | Requisitos mensuráveis e cenários independentes de ferramenta; sem scripts por padrão. |
| Implementar cenários de desempenho em k6 | `qa-k6` | Scripts e verificações pertinentes; geração não implica executar carga. |
| Analisar resultados de desempenho existentes | `qa-nft-plan` (interpretação de resultados) | Conclusões sustentadas pelas evidências; não iniciar nova execução automaticamente. |

Pedidos mistos: carregue e siga a skill de cada etapa pedida, uma de cada vez e na ordem do fluxo (`qa-analysis`, depois `qa-scenarios`, depois `qa-playwright`; ou `qa-nft-plan`, depois `qa-k6`), reaproveitando o resultado da anterior. Não faça você mesmo, sem a skill, o trabalho de uma etapa (por exemplo, escrever cenários ou código). Não pule uma etapa pedida, nem a refaça se já foi concluída; a implementação segue a seção 3. Mantenha rastreabilidade comum, mas use os critérios e formatos próprios de cada objetivo. Um contrato de API pode alimentar ambos os planejamentos; sua presença não determina a ferramenta. Para outra ferramenta de desempenho explicitamente escolhida, preservar a especificação e consultar a documentação dela na implementação, sem substituí-la por k6. Planejamento de desempenho não representa cobertura completa de segurança, acessibilidade ou outros atributos não funcionais.

Desempenho com ferramenta ainda indefinida: levantar requisitos e escrever cenários não depende dela; esclarecer a escolha antes de implementar, aproveitando o planejamento já feito.

## 3. Aplicar escopo e autorização

Antes de qualquer pergunta ou decisão sobre revisar os cenários, seguir para a automação ou implementar, **leia** [escopo, autorização e prontidão](../../shared/authorization-and-scope.md) e aplique-o. Cenários aprovados, por si só, não autorizam implementação.

Quando houver intenção de continuar o fluxo, mas a próxima etapa estiver indefinida, pergunte exatamente: `Você quer revisar os cenários ou seguir para a automação?`

Gerar scripts não autoriza executar carga: **leia** e aplique [execução de carga](../../shared/load-execution.md) antes de qualquer verificação que envie requisições.

Projetos executáveis ficam fora do toolkit; artefatos de QA vão para o workspace conforme [locais e artefatos](../../shared/artifacts-and-paths.md).

## 4. Entregar o resultado

Ao finalizar, informar:

- o que foi analisado;
- principais gaps e riscos;
- cenários ou arquivos gerados;
- cobertura principal;
- arquitetura usada, quando houver automação;
- próximos passos recomendados.

Em desempenho, informar também requisitos e perfis selecionados, pendências quantitativas e prontidão para implementação. Se houver execução ou análise de resultados, distinguir validade da execução, cumprimento das metas e limitações das evidências. Não declarar capacidade ou aprovação do produto com base apenas na geração dos scripts.
