# Boas práticas de modelagem e medição

Aplique ao escrever as fichas de cenário e ao preparar a implementação.

- Justifique população concorrente (modelo fechado) ou taxa de chegada independente das respostas (modelo aberto) conforme o uso real.
- Diferencie usuários, repetições da jornada, requisições e transações de negócio. Declare qual unidade a taxa controla e quantas operações compõem cada jornada.
- Em modelos fechados, a lentidão pode reduzir a taxa de novas operações. Considere esse efeito ao avaliar a demanda.
- Modele pausas de usuário onde fizerem parte da jornada, distinguindo-as do mecanismo que controla a chegada de carga.
- Observe capacidade e recursos do gerador. Demanda não entregue não comprova isoladamente saturação do sistema testado.
- Especifique como falhas de validação funcional participam da aprovação. Diferencie proporção de validações aprovadas de proporção de transações bem-sucedidas.
- Defina o tratamento de 429, timeouts, retries e erros funcionais sem ocultar falhas com respostas HTTP aparentemente válidas.
- Meça separadamente latência das operações e duração completa da jornada quando ambas forem requisitos.
- Use resultados por operação e fase para evitar que agregados escondam degradações. Não exponha credenciais ou dados pessoais nos identificadores das métricas.
- Garanta amostras suficientes por janela. Ausência de amostras ou fase não executada não comprova aprovação; percentis de amostras pequenas têm interpretação limitada.
- Um critério agregado não localiza precisamente ruptura ou recuperação. Use resultados por fase e séries temporais conforme a pergunta do teste.
- Diferencie critérios de interrupção de critérios de aprovação e explicite quando cada um será avaliado.
- Para recursos, autoscaling e filas, indique a fonte de observabilidade e a correlação com a execução. Não atribua ao gerador medições que ele não coleta.
- Registre versão, massa, ambiente, parâmetros e baseline. Preserve o status real da execução e suas evidências.
