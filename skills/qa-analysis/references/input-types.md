# Instruções por tipo de input

Use apenas a seção do tipo recebido. Em combinações, aplique todas as pertinentes e cruze as fontes.

## User Story ou requisitos

- Identificar o fluxo principal.
- Identificar fluxos alternativos.
- Verificar se os critérios de aceite são testáveis.
- Identificar regras implícitas.
- Apontar ambiguidades de negócio.

## OpenAPI, Swagger ou contrato de API

- Analisar endpoints, métodos e parâmetros.
- Validar status codes documentados.
- Identificar campos obrigatórios e opcionais.
- Avaliar schemas de request e response.
- Detectar inconsistências entre payloads, exemplos e descrições.
- Verificar erros esperados.

## Figma ou descrição de UI

- Identificar fluxos de usuário.
- Mapear estados de tela.
- Avaliar validações visuais e interações.
- Identificar mensagens, estados vazios, loading e erro.
- Apontar possíveis falhas de UX que impactem testes.

## Logs, payloads ou erros

- Identificar o contexto provável do problema.
- Separar evidência observada de hipótese.
- Mapear campos relevantes.
- Sugerir verificações adicionais.
- Indicar riscos de investigação incompleta.

## Métricas, incidentes e objetivos de desempenho

- Registrar valores, unidades, janelas de avaliação e ambiente como fatos, sem converter hipóteses em requisitos.
- Registrar ausências como pendências, sem usar valores de exemplo como metas reais.
- Para levantar requisitos e escrever cenários de desempenho, encaminhar à skill `qa-perf-plan`.
