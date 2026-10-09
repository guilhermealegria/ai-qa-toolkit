---
name: qa-playwright
description: Converte cenários funcionais em automação Playwright de API e UI em JavaScript, com Hybrid Architecture por padrão, em projeto fora do toolkit. Use somente quando a implementação foi solicitada ou autorizada.
---

# Automação Playwright

Gere testes automatizados em Playwright a partir de cenários de teste funcionais.

Referências desta skill, lidas somente quando necessárias:

- [Escopo, autorização e prontidão](../../shared/authorization-and-scope.md): leia antes de implementar.
- [Contrato dos cenários funcionais](../../shared/scenario-contract.md)
- [Linguagem e ecossistema](references/languages.md)
- [Arquitetura](references/architecture-guidelines.md)
- [Comandos no terminal](../../shared/terminal-commands.md)
- [Locais, artefatos, privacidade e versionamento](../../shared/artifacts-and-paths.md)

## Condições para implementação

Aplique [escopo, autorização e prontidão](../../shared/authorization-and-scope.md): a automação deve estar no escopo solicitado ou autorizado, com cenários disponíveis e lacunas relevantes identificadas. Respeite revisão solicitada ainda pendente e não repita autorização já dada. Receber cenários aprovados não amplia, por si só, um pedido de análise ou revisão.

## Entrada esperada

### Obrigatório

Cenários estruturados conforme o [contrato dos cenários funcionais](../../shared/scenario-contract.md), recebidos ou criados na preparação autorizada. Aceite qualquer um dos formatos:

- BDD em arquivo `.feature` ou texto na conversa.
- Step-by-step em tabela na conversa, documento, CSV ou planilha `.xlsx` legível.
- Outro formato estruturado que permita identificar contexto, dados, ações e resultados esperados.

Não exija `.feature` nem converta step-by-step para BDD como pré-condição para automatizar. A extensão do arquivo, sozinha, não garante que o conteúdo seja suficiente.

Antes da conversão, identifique a origem e o ID de cada cenário, as pré-condições, a sequência de ações, os dados e os resultados verificáveis. Preserve IDs existentes; quando ausentes, atribua IDs locais e informe o mapeamento. Registre lacunas e esclareça as que bloqueiem a implementação, avançando nos cenários independentes. Se não for possível ler a entrada, informe a limitação e solicite uma representação acessível, sem presumir seu conteúdo.

### Opcional

- OpenAPI (para detalhes técnicos).
- Regras adicionais.
- Análises anteriores e referências de requisitos para complementar a rastreabilidade.

## Linguagem

Use JavaScript por padrão; TypeScript ou outra linguagem suportada somente quando explicitamente solicitado. Se o projeto indicado usar outra linguagem e não houver escolha explícita, esclareça a divergência antes de adicionar código incompatível. O detalhe por ecossistema está em [linguagem e ecossistema](references/languages.md).

## Arquitetura

Se o usuário não especificar arquitetura, utilize **Hybrid Architecture**, combinando Page Object Model, Application Actions e Functional Abstractions. Consulte a [definição integrada](references/architecture-guidelines.md#4-hybrid-architecture--padrão-do-toolkit), com responsabilidades, dependências e composição por tipo de projeto. Crie apenas as camadas necessárias e respeite as convenções de projetos existentes.

## Instruções

### 1. Conversão de cenários em testes

Para cada cenário:

- Crie um teste correspondente no runner escolhido (`test()` em Playwright Test para Node.js).
- O nome do teste deve refletir o ID e o título do cenário.
- Mantenha rastreabilidade entre a origem, o cenário e o teste, sem exigir cópia dos artefatos de cenários para o projeto.

Mapeamento BDD:

- Given → setup e contexto.
- When → ação (request, ação de UI etc.).
- And → setup, ação ou assertion conforme seu papel no cenário.
- Then → assertions.

Mapeamento step-by-step e outros formatos estruturados:

- Pré-condições → setup e contexto.
- Dados → entradas e preparação de massa.
- Ações ordenadas → operações na sequência descrita.
- Resultados esperados → assertions nos pontos correspondentes, incluindo resultados intermediários relevantes.
- Linhas com o mesmo ID → passos de um único cenário; não gerar um teste isolado por linha.

### 2. Estrutura dos testes

- Agrupe por funcionalidade usando o mecanismo do runner escolhido (`test.describe` em Playwright Test para Node.js).
- Evite duplicação de código; crie helpers quando necessário, mantendo-os simples.
- Aplique as responsabilidades e a direção de dependências da [referência de arquitetura](references/architecture-guidelines.md).
- Use pages quando houver UI e services quando chamadas API se beneficiarem da abstração. Actions podem compor jornadas de UI, API ou ambas; crie-as somente quando houver benefício concreto.
- Mantenha as assertions principais nos testes e use fixtures para composição, preparação e limpeza quando necessário.
- Crie helpers e data apenas para lógica auxiliar e massa concretas; não crie pastas vazias para completar uma estrutura.

### 3. Testes de API (quando aplicável)

- Use o contexto HTTP do Playwright apropriado à linguagem e ao ciclo de vida do projeto (`request` no Playwright Test para Node.js).
- Verifique o status esperado pelo cenário e contrato; não substitua um status específico por uma checagem genérica de sucesso.
- Valide corpo, campos, headers e schema quando fizerem parte do comportamento esperado. Não interprete como JSON uma resposta sem corpo ou de outro formato; para ausência de corpo prevista no contrato, verifique essa ausência.
- Valide códigos e mensagens de erro quando o cenário os exigir, especialmente nos fluxos negativos. Não exija mensagem de erro em um fluxo de sucesso.
- Verifique efeitos de negócio, persistência ou conclusão assíncrona quando previstos no cenário; um status de aceite, sozinho, não comprova a conclusão do processamento.

Referência: [testes de API](https://playwright.dev/docs/api-testing).

### 4. Testes de UI (quando aplicável)

- Use locators baseados na semântica da interface, como papel e nome acessível, labels ou identificadores de teste acordados. Evite seletores dependentes de detalhes acidentais do DOM.
- Valide os resultados observáveis previstos no cenário: conteúdo, estado de controles, navegação ou mensagens relevantes. Não imponha status HTTP ou corpo de resposta a um cenário exclusivamente de UI.
- Use assertions de UI com espera automática da linguagem escolhida (`await expect(locator)...` em Playwright Test para Node.js); não troque a espera por condições por pausas fixas.
- Quando o cenário exigir validação de rede ou integração, acrescente a verificação API pertinente, mantendo explícito o resultado esperado pela interface.

Referências: [locators](https://playwright.dev/docs/locators) e [assertions](https://playwright.dev/docs/test-assertions).

## Integração com OpenAPI (se fornecido)

- Use o OpenAPI para endpoints, payloads e schemas.
- Não sobrescreva o comportamento definido nos cenários.
- Se houver conflito entre cenário e contrato, registre a divergência e esclareça o comportamento esperado antes de implementar a parte afetada.

## Boas práticas e qualidade obrigatórias

- Código legível e limpo, com assertions explícitas e sem dados sensíveis hardcoded.
- Use o modelo de execução correto da API escolhida; aguarde operações assíncronas quando aplicável, sem impor async/await a APIs síncronas.
- Cada teste deve verificar os resultados esperados do seu cenário, conforme as regras de API, UI ou ambas. Não invente assertions apenas para cumprir uma lista genérica.
- Cubra os cenários negativos mapeados na etapa de geração de cenários.
- Evite lógica complexa dentro dos testes; extraia para services, helpers ou actions.
- Garanta independência entre testes, sem dependência de ordem de execução.

## Local de criação do projeto

- Crie ou atualize o projeto Playwright no destino informado pelo usuário ou já definido na conversa, fora do repositório do toolkit e da instalação do plugin. Não escolha automaticamente uma pasta no home.
- Antes de escrever, resolva o caminho efetivo, incluindo links simbólicos existentes, e confira que ele não aponta para o toolkit ou para sua instalação. Se apontar, esclareça um destino externo. A exceção para exemplos e templates do próprio toolkit continua restrita a tarefas que os solicitem.
- Se não houver destino inequívoco para a escrita, pergunte onde criar ou atualizar o projeto; continue a análise e a preparação do conteúdo que não dependam dessa resposta.
- Em destino existente, leia suas instruções locais e inspecione configuração, dependências e estrutura antes de editar. Preserve alterações do usuário e integre os testes ao projeto, sem inicializar outro projeto por cima nem substituir configurações sem necessidade.
- Respeite o shell e as permissões de escrita do ambiente, conforme [comandos no terminal](../../shared/terminal-commands.md). Falta de acesso ao destino não autoriza gerar o projeto dentro do toolkit.
- Em pedidos de somente conteúdo ou proposta, apresente os arquivos e caminhos sugeridos sem criar diretórios nem exigir um destino local para prosseguir.

Regras gerais de locais e versionamento: [locais e artefatos](../../shared/artifacts-and-paths.md).

## Formato de saída

### 1. Resumo

Informe:

- quantidade de testes;
- cobertura;
- arquitetura utilizada;
- linguagem, runner e destino utilizados (ou propostos, quando não houver escrita);
- mapeamento entre IDs dos cenários e arquivos ou nomes dos testes;
- cenários não implementados e respectivas pendências.

### 2. Estrutura sugerida

Crie a estrutura de acordo com a arquitetura selecionada, consultando a [referência de arquitetura](references/architecture-guidelines.md), e explique quais camadas foram necessárias.

### 3. Código

- Gere arquivos completos com caminhos.
- Inclua os comandos de instalação, preparação e execução aplicáveis ao projeto, à linguagem e ao shell. Em um novo projeto Node.js com npm, por exemplo, indique `npm install` e `npx playwright test`; inclua a instalação dos navegadores necessários quando houver testes de UI. Em projetos existentes, priorize os comandos já definidos. Não prescreva npm a projetos Python, Java ou .NET.
- Respeite pedidos de somente conteúdo ou proposta de código; nesses casos, não declare arquivos criados.
- Se a escrita estiver indisponível, identifique o código como conteúdo proposto e informe os arquivos cuja criação permanece pendente.
- Distinga código gerado de código executado ou validado, informando verificações realmente realizadas e limitações.
