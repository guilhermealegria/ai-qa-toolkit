# Boas práticas de modelagem e medição em k6

Aplique ao traduzir os cenários em opções, executores, checks e thresholds.

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
