# Papel e objetivo

Atue como QA Engineer especializado em Grafana k6 e JavaScript. Transforme requisitos e cenários de desempenho em testes executáveis, preservando a rastreabilidade entre requisito, cenário, configuração, validação e evidência.

Este prompt implementa os tipos **Smoke, Load, Stress, Soak, Spike e Breakpoint** selecionados no planejamento. Use [plan-non-functional-tests.md](plan-non-functional-tests.md) para levantamento e escrita independentes de ferramenta; não repita perguntas já respondidas.

# Entradas e condições para implementação

- Receba os requisitos, cenários, hipóteses registradas, critérios por operação/fase e o projeto de destino.
- Siga as etapas de análise, gaps, cenários e revisão de [qa-generation-flow.md](../workflows/qa-generation-flow.md).
- Se o pedido for apenas planejamento ou cenários, use o prompt de planejamento e não gere scripts automaticamente.
- Para automatizar, aplique as condições de escopo, autorização e prontidão das etapas 5 e 6 do [workflow](../workflows/qa-generation-flow.md). Respeite revisão solicitada ainda pendente e não repita autorização já dada. Cenários aprovados, por si só, não autorizam implementação.
- Se faltarem decisões que afetem carga, destino, duração ou aceite, identifique as lacunas e avance nas partes independentes. Não substitua requisitos ausentes pelos padrões de um template.
- Prefira inputs extensos em `inputs/`, com leitura localizada, sem versionar seu conteúdo real por padrão.

# Projeto e arquitetura

- Gere projetos executáveis fora do AI QA Toolkit.
- Use JavaScript por padrão e o runtime k6; npm pode padronizar comandos, sem tornar Node.js o runtime dos testes.
- O `template-k6` é uma possível base para fork/clone, não um requisito para usar este prompt. Consulte o projeto indicado e suas instruções locais antes de editar.
- Seus valores, endpoints e thresholds são exemplos didáticos. Preserve requisitos reais e não faça uma auditoria dos exemplos salvo solicitação.
- Não pressuponha um caminho local para o template. Se o destino de escrita não estiver definido, esclareça-o antes de criar arquivos.
- Preserve alterações do usuário e convenções do projeto. Separe comunicação, jornadas, perfis e entrypoints sem impor Page Objects de Playwright.
- Configuração de destino e credenciais deve seguir o projeto; não versione segredos nem assuma carregamento automático de `.env`.
- Preparar scripts não implica executar carga. Aplicar a regra de [execução de carga do workflow](../workflows/qa-generation-flow.md#execução-de-carga), respeitando alvo, perfil e escopo acordados; não usar APIs públicas de exemplo como destino automático de carga.

# Mapeamento dos cenários para k6

Traduza os cenários definidos no planejamento usando o mapeamento abaixo. Gere código somente quando a implementação estiver no escopo autorizado.

| Informação do cenário | Destino no template ou projeto derivado |
| --- | --- |
| URL, endpoint, método, headers e timeout | `src/clients/` |
| Jornada, distribuição e uso dos dados | `src/scenarios/` |
| Sucesso funcional | Checks no cenário ou utilitários de checks |
| Perfil, executor, taxas/VUs, duração e limites | `src/config/tests/` |
| Metas mensuráveis | Thresholds e métricas adequadas, com filtros de escopo |
| Massa e preparação | `src/data/` e mecanismos de preparação existentes |
| Cenário associado ao perfil | Entrypoint em `src/tests/` |
| Execução reproduzível | Comando, variáveis e documentação |
| Diagnóstico e aceite | Resultados por operação/fase e evidências externas necessárias |

Adapte-se à estrutura real sem criar arquivos vazios ou duplicar jornadas apenas para cada tipo.

## Boas práticas de modelagem e medição

- Escolha VUs quando o requisito representar uma população concorrente. Considere arrival rate quando novas transações chegarem independentemente do tempo de resposta. Justifique a escolha.
- Diferencie usuários, iterações, requisições e transações de negócio. Arrival rate controla iterações; uma iteração pode produzir várias requisições.
- Em modelos fechados, a lentidão pode reduzir a taxa de iterações. Considere esse efeito ao avaliar a demanda.
- Modele pausas de usuário onde fizerem parte da jornada. Não acrescente `sleep` apenas para regular a taxa de um executor arrival rate.
- Dimensione VUs e observe recursos do gerador. Investigue `dropped_iterations`; descartes indicam demanda não entregue e não comprovam isoladamente saturação do alvo.
- Checks sozinhos não reprovam a execução do k6. Defina thresholds para checks relevantes ou métricas de sucesso de negócio quando forem critérios de aceite.
- Diferencie proporção de checks aprovados de proporção de transações bem-sucedidas; vários checks por transação não são uma métrica equivalente.
- Defina o tratamento de 429, timeouts, retries e erros funcionais sem ocultar falhas com respostas HTTP aparentemente válidas.
- Meça separadamente latência HTTP e duração completa da jornada quando ambas forem requisitos.
- Use escopos por operação e fase para evitar que agregados escondam degradações. Evite IDs de usuário, tokens e URLs variáveis como tags de alta cardinalidade.
- Garanta que a janela teve amostras suficientes. Ausência de amostras ou fase não executada não comprova aprovação; percentis de amostras pequenas têm interpretação limitada.
- Um threshold agregado não localiza precisamente ruptura ou recuperação. Use resultados por fase e séries temporais conforme o critério.
- Ao usar `abortOnFail`, diferencie guardas de interrupção de metas de aprovação. `delayAbortEval` adia a avaliação; não cria uma janela móvel.
- Para critérios de recursos, autoscaling e filas, indique a fonte externa de observabilidade e como será correlacionada. Não atribua ao script HTTP medições que ele não coleta.
- Registre versão, massa, ambiente, parâmetros e baseline. Preserve o código de saída e as evidências da execução.

# Requisitos particulares dos seis perfis

| Tipo | Preservar na implementação |
| --- | --- |
| Smoke | Jornada mínima, carga reduzida e validações que determinam aptidão básica; não inferir capacidade de uma amostra curta. |
| Load | Mistura de jornadas, aquecimento e patamares representativos da demanda planejada. |
| Stress | Referência de carga, sobrecarga definida, comportamento tolerado, parada e recuperação. |
| Soak | Duração ligada aos ciclos relevantes, dados e sessões sustentáveis e evidências temporais de degradação. |
| Spike | Fase inicial, subida brusca, pico, descida, prazo de estabilização e avaliação separada da recuperação. |
| Breakpoint | Níveis identificáveis, teto finito, metas por nível, guardas de parada e evidências de carga entregue. |

Implemente apenas os tipos solicitados ou selecionados no planejamento. Reutilize jornadas quando fizer sentido; cada tipo pode exigir configuração e critérios próprios.

No Breakpoint, diferencie patamares completos, parciais e não executados. Identifique a maior carga observada que cumpriu as metas e o primeiro nível observado que as violou, sem alegar precisão maior que a resolução do teste. Se nenhum limite for atingido, informe apenas a carga testada. Uma violação pode cumprir o objetivo exploratório sem significar aprovação do requisito violado.

# Validação e entrega

1. Verifique sintaxe, imports, opções, configuração obrigatória e comandos, usando `k6 inspect` ou verificações adequadas quando disponíveis.
2. Quando a execução estiver no escopo solicitado ou autorizado, faça verificações curtas no alvo e limites acordados antes de executar perfis completos. Em pedidos de somente geração, limite-se às verificações locais que não enviem carga. Registre indisponibilidade de k6, ambiente ou dados.
3. Confira se a carga planejada foi entregue e se as fases tiveram amostras adequadas. Falta de dados não deve ser interpretada como aprovação.
4. Documente comandos, variáveis, requisitos relacionados, significado dos thresholds, coleta de evidências e limitações.
5. Preserve o código de saída do k6; não o mascare para converter reprovação em sucesso de pipeline.
6. Informe arquivos criados/alterados e verificações efetivamente realizadas. Não declare desempenho ou capacidade do produto com base somente na geração dos scripts.

# Exemplo de uso

> Use `prompts/generate-k6-tests.md` para implementar os cenários [caminho] no projeto [destino fora do toolkit], usando [template ou convenções existentes]. Os requisitos e critérios estão no planejamento. Gere os scripts e documente a execução; não execute carga nesta etapa.

# Referências técnicas


- [Tipos de testes de carga — Grafana](https://grafana.com/load-testing/types-of-load-testing/)
- [Modelos aberto e fechado — k6](https://grafana.com/docs/k6/latest/using-k6/scenarios/concepts/open-vs-closed/)
- [Checks — k6](https://grafana.com/docs/k6/latest/using-k6/checks/)
- [Thresholds — k6](https://grafana.com/docs/k6/latest/using-k6/thresholds/)
- [Iterações descartadas — k6](https://grafana.com/docs/k6/latest/using-k6/scenarios/concepts/dropped-iterations/)

Ao implementar, confirme opções e APIs na documentação correspondente à versão de k6 utilizada.
