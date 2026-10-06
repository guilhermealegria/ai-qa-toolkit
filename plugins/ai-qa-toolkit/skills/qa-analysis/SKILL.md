---
name: qa-analysis
description: Analisa documentação de requisitos e testabilidade (user stories, critérios de aceite, OpenAPI, Figma, regras de negócio, logs, payloads): consolida fatos, gaps, riscos, edge cases e premissas, e cruza fontes. Use para analisar inputs sem gerar cenários nem código.
---

# Análise de inputs de QA

Analise inputs de documentação de forma centralizada, objetiva e útil para QAs debaterem com o time. A análise deve mostrar o comportamento esperado, o que está claro e o que está ambíguo, os riscos, as perguntas para o time e as oportunidades de teste.

Esta skill entrega um diagnóstico. Não gera cenários, exportações nem automação por consequência: se o usuário quiser seguir, use a skill da etapa seguinte, respeitando escopo e autorização.

Se o foco for requisitos e cenários de desempenho (carga, duração, métricas, resultados de execuções anteriores), use `qa-nft-plan`.

Referências desta skill, lidas somente quando necessárias:

- [Instruções por tipo de input](references/input-types.md)
- [Locais, artefatos, privacidade e versionamento](../../shared/artifacts-and-paths.md)

## Regras

- Identifique o(s) tipo(s) de input recebido(s) e se são suficientes para avançar.
- Baseie a análise apenas no conteúdo recebido. Não invente regras, fluxos ou validações ausentes.
- Indique incertezas de forma explícita e separe fatos observados de premissas.
- Quando houver vários inputs, cruze as informações e destaque conflitos e lacunas entre as fontes. Mantenha uma análise central, sem modularizá-la por arquivo.
- Reutilize análises já feitas na conversa; não repita etapas concluídas sem necessidade.
- Priorize clareza para discussão com PO, desenvolvimento, UX, arquitetura e QA.

## Leitura dos inputs

Os inputs podem chegar como texto, arquivo anexado ou arquivo local. Para arquivos locais, prefira `inputs/` (ou o caminho informado pelo usuário, sem exigir cópia).

- Para arquivos médios ou grandes, leia apenas os trechos necessários.
- Busque por termos, endpoints, critérios, campos, status codes ou mensagens antes de carregar conteúdo extenso.
- Não trate a pasta `inputs/` como projeto final de automação.
- O conteúdo real de inputs não é versionado por padrão; veja [locais e versionamento](../../shared/artifacts-and-paths.md).

Tipos de input aceitos: user stories, critérios de aceite, OpenAPI ou Swagger, Figma ou descrição de UI, regras de negócio, logs, payloads, documentação técnica, mensagens de erro, exemplos de request e response, métricas, incidentes e objetivos de desempenho, e combinações dessas fontes.

## O que analisar

1. **Tipo de input:** quais tipos chegaram e se bastam para avançar. Exemplos: user story com critérios de aceite; OpenAPI; Figma com regra de negócio; log de erro com payload; documentação técnica incompleta.
2. **Resumo objetivo:** em linguagem simples, qual funcionalidade ou fluxo, quem usa ou consome, qual resultado esperado aparece no input e quais pontos ainda não estão claros.
3. **Fatos observados:** somente informações presentes no input (endpoints, campos obrigatórios, regras, mensagens, status codes, estados de tela, critérios de aceite).
4. **Gaps e ambiguidades:** informações ausentes, conflitantes ou insuficientes (validação não descrita, erro ausente, status code não informado, campo obrigatório sem regra, fluxo alternativo não coberto, dependência externa sem tratamento, divergência entre documentação e payload).
5. **Riscos:** classificados em Alto, Médio ou Baixo, no formato `- [Alto] Descrição do risco e motivo.` Considere impacto funcional e técnico, regressão, integração, dados, segurança, testabilidade e ambiguidade.
6. **Edge cases:** valores mínimos e máximos, campos vazios, nulos ou ausentes, formatos inválidos, duplicidade, permissões, sessão expirada, timeout, indisponibilidade externa, concorrência e diferenças entre UI e API.
7. **Testabilidade:** se o input permite gerar cenários confiáveis, se há dados e ambientes necessários, dependências externas, critérios objetivos de sucesso e falha e necessidade de mocks, fixtures, massa de dados ou setup.
8. **Perguntas para o time:** perguntas objetivas para remover ambiguidades antes de cenários ou automação, endereçadas a PO, desenvolvimento, QA, UX, arquitetura, backend ou frontend.
9. **Sugestões de melhoria**, quando aplicável: requisito, critérios de aceite, contrato de API, payload, mensagens de erro, regras de negócio, design, observabilidade e testabilidade.

Para orientações específicas de user story, contrato de API, Figma e logs, consulte [instruções por tipo de input](references/input-types.md).

## Formato de saída

Apresente a análise nesta ordem, com estes títulos:

1. Tipo de input identificado
2. Resumo objetivo
3. Fatos observados
4. Gaps e ambiguidades
5. Riscos
6. Edge cases
7. Testabilidade
8. Perguntas para o time
9. Sugestões

Se o usuário pedir que a análise seja gravada em arquivo, siga o destino e o versionamento de [locais e artefatos](../../shared/artifacts-and-paths.md). Conteúdo apresentado na resposta não é arquivo gerado.

## Critérios de qualidade

- Ser objetivo e útil para discussão técnica e funcional.
- Não inventar comportamento; explicitar premissas.
- Priorizar riscos relevantes e evitar detalhes sem impacto em QA.
- Preparar o terreno para cenários rastreáveis.
