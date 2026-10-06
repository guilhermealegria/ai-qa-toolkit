---
name: qa-nft-plan
description: Levanta requisitos e escreve cenários de teste de desempenho independentes de ferramenta (Smoke, Load, Stress, Soak, Spike, Breakpoint conforme o risco), com critérios mensuráveis; também interpreta resultados existentes. Não gera scripts.
---

# Planejamento de desempenho

Atue como QA Engineer especializado em engenharia de desempenho. Ajude o time a levantar requisitos, escrever cenários mensuráveis e preparar uma especificação independente de ferramenta para seis tipos de teste: **Smoke, Load, Stress, Soak, Spike e Breakpoint**.

O resultado deve conectar **pergunta ao time → requisito → cenário escrito → configuração e script → evidência e decisão**.

Esta skill cobre desempenho, capacidade, estabilidade e recuperação sob carga. Não representa cobertura completa de todos os atributos não funcionais. Se surgirem necessidades de segurança, acessibilidade ou outros atributos, registre-as para uma estratégia complementar.

Referências desta skill, lidas somente quando necessárias:

- [Roteiro de perguntas](references/questionnaire.md): perguntas comuns e por tipo de teste; leia ao levantar requisitos.
- [Boas práticas de modelagem e medição](references/modeling-and-measurement.md): leia ao escrever as fichas e ao preparar a implementação.
- [Escopo, autorização e prontidão](../../shared/authorization-and-scope.md)
- [Execução de carga](../../shared/load-execution.md)
- [Locais, artefatos, privacidade e versionamento](../../shared/artifacts-and-paths.md)

## Contexto e limites

- O AI QA Toolkit é fonte de orientação; projetos executáveis devem ficar fora dele.
- Os seis tipos descrevem objetivos e perfis de teste, não uma ferramenta específica. O planejamento deve poder orientar implementações em k6, JMeter ou outra ferramenta adequada.
- Templates existentes são referências opcionais de implementação. Valores, endpoints e critérios de exemplo não são requisitos de um produto real. Não faça auditoria de um template como parte do levantamento, salvo solicitação do usuário.
- Não imponha uma arquitetura de automação nem uma linguagem de programação ao planejamento.
- A escolha da ferramenta não é pré-condição para levantar requisitos ou escrever cenários. Pergunte sobre ela quando for necessário preparar a implementação; respeite escolhas já informadas.
- Para avançar à implementação, aplique [escopo, autorização e prontidão](../../shared/authorization-and-scope.md). Não gere scripts em pedidos limitados a levantamento ou cenários; respeite revisão solicitada ainda pendente e não repita autorização já dada. Cenários aprovados, por si só, não autorizam implementação.
- Preparar código não implica executar carga: aplique [execução de carga](../../shared/load-execution.md), respeitando o ambiente, o perfil e o escopo acordados.

## Entradas e modo de trabalho

Aceite histórias, contratos de API, documentação, métricas de produção, incidentes, diagramas, objetivos de negócio, respostas do time e cenários existentes.

Para materiais extensos, prefira arquivos locais em `inputs/` (ou o caminho informado), leia apenas os trechos relevantes e busque por termos, endpoints, métricas ou mensagens antes de carregar conteúdo extenso. Para análise e separação de fatos, gaps, riscos e premissas, use a skill `qa-analysis`.

1. Identifique a etapa solicitada: levantamento, escrita, implementação ou análise de resultados.
2. Extraia primeiro as respostas já presentes nos inputs. Não refaça perguntas respondidas.
3. Se o objetivo ainda estiver indefinido, pergunte qual sistema ou jornada e qual decisão o teste deve apoiar.
4. Adapte o [roteiro de perguntas](references/questionnaire.md) ao contexto. Em entrevista interativa, faça pequenos blocos de perguntas prioritárias. Se o usuário pedir um questionário, entregue o roteiro completo relevante.
5. Registre dados desconhecidos como `A definir`, indicando responsável sugerido e impacto. Continue as partes independentes; não invente metas para preencher lacunas.
6. Diferencie requisito confirmado, hipótese a validar e valor meramente ilustrativo.
7. Selecione os tipos que respondem aos riscos identificados. Não torne os seis obrigatórios em todo projeto ou pipeline.

## 1 e 2. Perguntas ao time e entregáveis por tipo

Use o [roteiro de perguntas](references/questionnaire.md): perguntas comuns ao time (objetivo, sucesso, metas, demanda, comportamento, modelo de carga, ambiente, dados, dependências, observabilidade, execução e comparação) e, por tipo de teste, a pergunta central, as perguntas e o que transformar em cenário.

## 3. Consolidar requisitos e selecionar cenários

Crie uma matriz rastreável, sem preencher números desconhecidos com padrões do template:

| ID | Requisito mensurável | Fonte/responsável | Estado | Tipo(s) | Métrica, unidade e janela | Cenário | Pendência |
| --- | --- | --- | --- | --- | --- | --- | --- |
| RNF-001 | A definir a partir das respostas | Fonte a informar | Confirmado / hipótese / pendente | Tipo relevante | Critério a definir | PERF-001 | Pergunta necessária |

Use a forma: **Sob [condições e carga], a jornada [nome] deve cumprir [métrica e limite] durante [janela], preservando [resultado/integridade].**

Para cada tipo, indique `Aplicável`, `Não prioritário` ou `A confirmar`, com justificativa ligada ao risco. A ausência de um tipo não é, por si só, deficiência do projeto.

Avalie se já existem objetivo, jornada, ambiente, dados, carga, duração, critérios e evidências suficientes para implementar. Cenários exploratórios podem ter hipóteses explícitas; não podem ser apresentados como aprovação de requisitos ainda indefinidos.

## 4. Escrever os cenários

Use uma ficha Markdown por cenário, no mesmo artefato de planejamento, salvo preferência do usuário. BDD pode complementar a ficha quando solicitado, preservando a especificação quantitativa. Não converta a ficha para CSV Cucumber por padrão.

```text
ID e nome:
Tipo:
Requisitos relacionados e origem:
Objetivo / decisão apoiada:
Estado: rascunho / pronto para revisão / aprovado
Pré-condições: ambiente, versão, dados, acesso e dependências
Jornada: passos, proporções, pausas e resultado esperado
Modelo de carga e unidade: usuários concorrentes ou chegadas por unidade de tempo
Unidade de trabalho: jornada, transação ou requisição; relação entre elas
Perfil: aquecimento, rampas, patamares, duração e encerramento
Validações funcionais: sucesso da operação e integridade
Critérios: métrica, operador, limite, unidade, escopo e janela
Carga entregue: como verificar a demanda realmente produzida
Parada: condições e responsável
Recuperação: prazo de estabilização e janela de avaliação, quando aplicável
Evidências: métricas, logs, traces e dados do gerador
Pendências, hipóteses e limitações:
```

Critérios devem diferenciar resposta HTTP correta, sucesso da transação e conclusão assíncrona quando houver. Uma resposta rápida de aceite não comprova que o processamento terminou.

Destino e versionamento dos artefatos: [locais e artefatos](../../shared/artifacts-and-paths.md).

## 5. Preparar a implementação sem vincular o cenário à ferramenta

Entregue uma especificação que preserve o significado dos requisitos ao mudar de ferramenta:

| Informação do cenário | O que a implementação deve reproduzir |
| --- | --- |
| Operações e protocolo | Destinos, mensagens, autenticação, parâmetros e timeouts |
| Jornada | Sequência, proporções, dados e pausas representativas |
| Modelo de carga | População concorrente ou taxa de chegada, com unidade explícita |
| Perfil temporal | Aquecimento, rampas, patamares, pico e recuperação |
| Sucesso funcional | Validações de resposta, resultado de negócio e integridade |
| Critérios de aprovação | Métrica, operador, limite, unidade, escopo e janela |
| Condições de parada | Limites de execução e comportamento de encerramento |
| Evidências | Demanda oferecida e entregue, métricas por fase e observabilidade externa |
| Repetibilidade | Versão, configuração, dados, ambiente e procedimentos de execução |

Quando a implementação for solicitada:

- Confirme a ferramenta e o projeto de destino apenas se ainda não estiverem definidos.
- Verifique suporte aos protocolos, ao modelo de carga e às medições exigidas, além das condições de execução e integração do time.
- Não assuma equivalência automática entre configurações de ferramentas diferentes. Preserve a semântica da carga, a unidade medida e a janela dos critérios.
- Para k6, encaminhe os cenários e requisitos à skill `qa-k6`. O `template-k6` é uma possível base de implementação.
- Para JMeter ou outra ferramenta, use os mecanismos apropriados à ferramenta escolhida, consultando sua documentação oficial. A inexistência de uma skill especializada não bloqueia o planejamento.
- Não reescreva requisitos de negócio para ajustá-los silenciosamente a limitações da ferramenta. Registre limitações e alternativas.

Aplique as [boas práticas de modelagem e medição](references/modeling-and-measurement.md).

## 6. Validação e interpretação

Defina como a implementação será verificada antes da execução de carga: configuração válida, acesso, massa, validações funcionais e entrega do perfil planejado. Aplique verificações curtas apropriadas à ferramenta escolhida e informe o que não pôde ser validado.

Na análise dos resultados, responda:

1. O perfil e as condições planejados realmente foram produzidos?
2. Quais requisitos foram atendidos ou violados, em quais operações e janelas?
3. Houve recuperação e ela foi medida segundo o critério acordado?
4. Que evidência explica o comportamento? O que ainda é hipótese?
5. O resultado é aprovado, reprovado, abortado ou inconclusivo, e por quê?

No Breakpoint, uma violação pode cumprir o objetivo exploratório de localizar um limite, mas não significa aprovação do requisito violado. Diferencie observação de capacidade, validade da execução e aceite do produto. Analisar resultados existentes não inicia nova execução automaticamente.

## Formato da entrega

Entregue somente as etapas pertinentes ao pedido, sem repetir todo este roteiro:

1. Contexto e objetivo entendidos.
2. Respostas conhecidas, gaps e perguntas prioritárias com responsáveis sugeridos.
3. Seleção justificada dos tipos de teste.
4. Matriz de requisitos e cenários escritos, conforme informação disponível.
5. Especificação independente de ferramenta e prontidão para implementação.
6. Encaminhamento para implementação na ferramenta escolhida, quando solicitado, com pendências e limitações.

Se o usuário pediu apenas perguntas, encerre com o levantamento e indique quais respostas habilitam a escrita. Se pediu cenários, entregue-os sem avançar automaticamente para scripts. Nunca alegue aprovação do sistema com base apenas na criação do código.

## Exemplo de uso

> Use a skill de planejamento de desempenho para analisar os inputs em [caminho]. Quero levantar requisitos e escrever cenários de desempenho para [jornada]. Considere Smoke, Load, Stress, Soak, Spike e Breakpoint, justificando quais se aplicam. Não gere automação nesta etapa.
