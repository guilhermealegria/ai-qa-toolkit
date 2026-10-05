---
name: qa-flow
description: Coordena pedidos de QA que envolvem mais de uma etapa ou cuja intenção não está clara. Identifica o objetivo, aplica escopo e autorização e encaminha para análise, cenários, Playwright, planejamento de desempenho ou k6. Use quando houver apenas arquivos de requisitos, pedidos mistos ou dúvida sobre a próxima etapa.
---

# Coordenação do fluxo de QA

Esta skill decide qual capacidade usar e até onde avançar. Não analisa, não escreve cenários e não gera código: encaminha para a skill própria de cada etapa e respeita o escopo pedido.

Se o pedido já indicar uma única capacidade com clareza, use direto a skill dela, sem passar por aqui.

Referências desta skill, lidas somente quando necessárias:

- [Escopo, autorização e prontidão](references/shared/authorization-and-scope.md)
- [Execução de carga](references/shared/load-execution.md)
- [Locais, artefatos, privacidade e versionamento](references/shared/artifacts-and-paths.md)

## 1. Identificar a intenção

Primeiro verificar se o objetivo está definido na mensagem ou no contexto da conversa. Se já estiver, prosseguir nesse escopo sem perguntar de novo, inclusive quando não houver texto após "My request for Code:". Novos arquivos enviados depois desse pedido são inputs dele.

Quando não houver intenção identificável na mensagem nem no contexto, perguntar qual tarefa executar, oferecendo as opções da tabela da seção 2. Isso inclui mensagens com apenas arquivos, trechos ou links, em qualquer interface.

- Se a intenção estiver parcialmente definida (por exemplo, o tipo de entrega ou a ferramenta de desempenho), perguntar somente o que falta.
- Enquanto a intenção não estiver clara, não avançar para análise, cenários ou automação, nem inferir o objetivo apenas pelo tipo do arquivo. É permitido identificar o tipo dos inputs para oferecer opções adequadas, sem ler conteúdo além do necessário.

## 2. Encaminhar

Aplicar somente as etapas pertinentes ao pedido, reutilizando análises, cenários e decisões existentes.

| Objetivo solicitado | Skill | Entrega e limite |
| --- | --- | --- |
| Analisar documentação e testabilidade | `qa-analysis` | Diagnóstico; não iniciar implementação automaticamente. |
| Escrever ou revisar cenários funcionais | `qa-scenarios` | Cenários e exportações pertinentes ao pedido. |
| Automatizar testes funcionais de API/UI | `qa-playwright` | Implementação a partir de cenários, conforme escopo e autorização. |
| Levantar requisitos ou escrever cenários de desempenho | `qa-perf-plan` | Requisitos mensuráveis e cenários independentes de ferramenta; sem scripts por padrão. |
| Implementar cenários de desempenho em k6 | `qa-k6` | Scripts e verificações pertinentes; geração não implica executar carga. |
| Analisar resultados de desempenho existentes | `qa-perf-plan` (interpretação de resultados) | Conclusões sustentadas pelas evidências; não iniciar nova execução automaticamente. |

Pedidos mistos: manter rastreabilidade comum, mas usar os critérios e formatos próprios de cada objetivo. Um contrato de API pode alimentar ambos os planejamentos; sua presença não determina a ferramenta. Para outra ferramenta de desempenho explicitamente escolhida, preservar a especificação e consultar a documentação dela na implementação, sem substituí-la por k6. Planejamento de desempenho não representa cobertura completa de segurança, acessibilidade ou outros atributos não funcionais.

Desempenho com ferramenta ainda indefinida: levantar requisitos e escrever cenários não depende dela; esclarecer a escolha antes de implementar, aproveitando o planejamento já feito.

## 3. Aplicar escopo e autorização

Antes de passar de análise ou cenários para implementação, aplicar [escopo, autorização e prontidão](references/shared/authorization-and-scope.md). Cenários aprovados, por si só, não autorizam implementação.

Gerar scripts não autoriza executar carga: aplicar [execução de carga](references/shared/load-execution.md).

Projetos executáveis ficam fora do toolkit; artefatos de QA vão para o workspace conforme [locais e artefatos](references/shared/artifacts-and-paths.md).

## 4. Entregar o resultado

Ao finalizar, informar:

- o que foi analisado;
- principais gaps e riscos;
- cenários ou arquivos gerados;
- cobertura principal;
- arquitetura usada, quando houver automação;
- próximos passos recomendados.

Em desempenho, informar também requisitos e perfis selecionados, pendências quantitativas e prontidão para implementação. Se houver execução ou análise de resultados, distinguir validade da execução, cumprimento das metas e limitações das evidências. Não declarar capacidade ou aprovação do produto com base apenas na geração dos scripts.
