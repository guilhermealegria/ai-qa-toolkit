# Roteiro de perguntas

Use este roteiro no levantamento de requisitos de desempenho, adaptando ao contexto. Em entrevista interativa, faça pequenos blocos de perguntas prioritárias; se o usuário pedir um questionário, entregue o roteiro completo relevante.

## 1. Perguntas comuns ao time

| Tema | Perguntas a adaptar | Responsável sugerido | Decisão produzida |
| --- | --- | --- | --- |
| Objetivo | Qual decisão queremos tomar? Qual impacto da lentidão ou indisponibilidade? Quais jornadas são críticas? | Produto e negócio | Prioridade e escopo |
| Sucesso | O que confirma a conclusão correta de cada operação além do status HTTP? Há processamento assíncrono? | Produto e desenvolvimento | Validações funcionais, integridade e conclusão da jornada |
| Metas | Quais p95/p99, taxa de erro e vazão de sucesso são aceitáveis, por operação e jornada? Qual fonte sustenta as metas? | Produto, arquitetura e operações | Critérios mensuráveis |
| Demanda | Quais volumes normal e de pico, sazonalidade e crescimento esperado? Temos logs ou métricas que sustentem esses números? | Operações e dados | Carga e sua origem |
| Comportamento | Quais proporções entre operações, perfis, pausas, abandono, retries e duração das sessões? | Produto e desenvolvimento | Modelo das jornadas |
| Modelo de carga | Há população fixa de usuários ou novas transações chegando independentemente da lentidão? Quantas chamadas cada transação faz? | QA e arquitetura | Usuários concorrentes ou taxa de chegada |
| Ambiente | Como ambiente, rede, banco, cache, réplicas e autoscaling diferem de produção? Há recursos compartilhados? | Infraestrutura e arquitetura | Condições e limites da conclusão |
| Dados e acesso | Quais volumes de dados, payloads, usuários, permissões, tokens e ciclos de sessão? Como preparar, renovar e limpar a massa? | QA e desenvolvimento | Pré-condições e repetibilidade |
| Dependências | Quais serviços serão reais ou simulados? Existem rate limits, timeouts e filas? Como tratar 429 e retries? | Arquitetura e desenvolvimento | Fronteira do teste e classificação das falhas |
| Observabilidade | Teremos métricas, logs e traces de aplicação, banco, filas e gerador? Como correlacionar com cada execução? | Operações e QA | Evidências e diagnóstico |
| Execução | Qual alvo pode receber carga, em qual janela, com quais limites e condições de parada? Quem acompanha? | Responsáveis pelo ambiente | Plano de execução |
| Comparação | Qual baseline, versão e configuração registrar? Que repetição e tolerância à variação são necessárias? Quem aceita o resultado? | QA e engenharia | Regressão, rastreabilidade e aceite |

## 2. Perguntas e entregáveis por tipo

### Smoke — aptidão básica

**Pergunta central:** o fluxo, a massa, o acesso e o script estão aptos a sustentar os próximos testes?

- Quais operações mínimas devem ser exercitadas?
- Quais verificações de autenticação, dados e resultado precisam passar?
- Qual pequena carga e quantas iterações ou qual duração são suficientes?
- Qual falha impede a continuação e existe um limite básico de latência?

**Transformar em:** jornada mínima, carga reduzida, término definido e critérios funcionais/de desempenho básicos. Não usar uma amostra curta como evidência de capacidade ou estabilidade.

### Load — demanda esperada

**Pergunta central:** o sistema atende às metas sob a demanda planejada?

- Qual carga normal e qual pico esperado devemos sustentar? Em quais unidades?
- Qual distribuição entre jornadas representa o uso real?
- Quanto tempo precisamos de aquecimento, rampa e patamar estável?
- Por quanto tempo a demanda real permanece nesse nível?
- Quais metas se aplicam a cada operação e ao fluxo completo?

**Transformar em:** perfil representativo com patamares definidos, duração justificada e critérios por jornada/operação. Registrar como a demanda foi estimada.

### Stress — sobrecarga planejada

**Pergunta central:** como o sistema se comporta acima da carga habitual ou projetada?

- Qual referência permite chamar essa carga de sobrecarga?
- Quais níveis acima da referência serão exercitados e por quanto tempo?
- Sob pressão, são aceitáveis filas, rejeição controlada ou degradação parcial?
- Que propriedades devem continuar válidas, como ausência de perda ou duplicidade?
- Quais condições encerram o teste?
- Como e em quanto tempo o sistema deve se recuperar após a redução?

**Transformar em:** níveis de sobrecarga, critérios específicos sob pressão, guardas de interrupção e fase de recuperação. Não assumir que um número fixo de usuários concorrentes caracteriza stress em qualquer sistema.

### Soak — estabilidade prolongada

**Pergunta central:** o sistema mantém seu comportamento durante um período relevante de operação?

- Qual carga precisa ser mantida e por quanto tempo?
- Quais ciclos devem ocorrer: expiração de token, tarefas agendadas, renovação de conexões ou limpeza de cache?
- Quais degradações procuramos: memória, conexões, filas, latência ou erros crescentes?
- Como garantir dados suficientes e sessões válidas durante toda a execução?
- Quais tendências e diferenças entre início, meio e fim são aceitáveis?

**Transformar em:** duração vinculada aos ciclos, massa sustentável, monitoramento temporal e critérios de estabilidade. Não escolher duração apenas pelo nome do teste ou aprovar usando somente o agregado final.

### Spike — pico abrupto

**Pergunta central:** como o sistema absorve uma mudança repentina de demanda e se recupera?

- Qual evento provoca o pico? Qual carga inicial e qual amplitude?
- Em quanto tempo a demanda sobe e desce?
- Quanto tempo o pico dura e quantas vezes se repete?
- Quais erros e latências são aceitáveis durante o pico?
- Qual prazo de estabilização e quais critérios comprovam recuperação?

**Transformar em:** fase inicial, subida brusca, pico, descida e observação posterior. Diferenciar prazo permitido para estabilizar da janela em que as metas devem ser cumpridas; avaliar pico e recuperação separadamente.

### Breakpoint — descoberta do limite

**Pergunta central:** em qual faixa de carga observada o sistema deixa de cumprir as metas?

- Qual limite procuramos: latência, erros, vazão, integridade ou saturação?
- Qual carga inicial, incremento e duração de cada patamar?
- Qual teto finito de carga e de duração?
- Quais critérios indicam violação das metas e quais exigem interrupção imediata?
- Como comprovar que o gerador entregou a demanda e não foi o limitante?
- Como repetir/refinar as etapas próximas à violação?

**Transformar em:** patamares identificáveis, critérios por nível, guardas de parada e evidências da demanda oferecida e entregue. Stress avalia sobrecarga planejada; Breakpoint procura uma faixa de limite por progressão.

Não declarar capacidade máxima se nenhum limite for atingido. Reportar maior nível completo observado que atendeu às metas, primeiro nível observado que as violou e limitações da estimativa. Separar fases parciais, não executadas e conclusões inconclusivas.
