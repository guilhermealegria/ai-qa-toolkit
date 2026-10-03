# Objetivo

Definir padrões arquiteturais, convenções e boas práticas para automação de testes utilizando Playwright.

Este documento deve ser utilizado como referência principal para geração automatizada de código via IA.

As estruturas abaixo pertencem ao projeto de automação gerado fora do toolkit. Elas não devem ser criadas neste repositório, exceto para exemplos ou templates solicitados. Estas convenções se aplicam a Playwright; não devem ser impostas a projetos k6.

---

# Princípios gerais

## Objetivos da automação

A automação deve priorizar:

- Legibilidade
- Reutilização
- Baixo acoplamento
- Facilidade de manutenção
- Escalabilidade
- Clareza de responsabilidade

---

# Arquiteturas suportadas

## 1. Page Object Model (POM)

### Quando usar
- Projetos pequenos e médios
- Fluxos simples
- UI-centric automation

### Estrutura

```text
pages/
tests/
fixtures/
helpers/
```

### Responsabilidades

```text
tests/: cenários e assertions
pages/: seletores e ações da tela
fixtures/: setup compartilhado
helpers/: funções auxiliares
data/: massa de teste
```

### Regras

```text
Pages não devem conter regra de negócio complexa
Pages não devem concentrar muitas assertions
Tests devem continuar legíveis e objetivos
```

## 2. Application Actions

### Quando usar
- Fluxos complexos
- Regras de negócio complexas
- E2E robusto

### Estrutura

```text
pages/
actions/
tests/
fixtures/
helpers/
```

### Responsabilidades

```text
pages/: interações atômicas com a UI
actions/: fluxos de negócio reutilizáveis
tests/: validações e orquestração final
fixtures/: setup e contexto
data/: dados de teste
```

### Regras

```text
Actions não devem ter assertions principais
Actions devem representar intenção de negócio
Tests devem ler como uma especificação executável
```

## 3. Functional Abstractions

### Quando usar
- Fluxos complexos
- Regras de negócio complexas
- E2E robusto

### Estrutura

```text
helpers/
services/
tests/
```

### Responsabilidades

```text
tests/: cenários e assertions
services/: chamadas HTTP/API
helpers/: criação de payloads, schemas, utilitários
data/: massa de teste
fixtures/: setup de contexto
```

### Regras

```text
Services devem encapsular chamadas técnicas
Helpers devem ser pequenos e reutilizáveis
Tests não devem montar payloads complexos diretamente
```

## 4. Hybrid Architecture — padrão do toolkit

Usar quando o usuário não especificar outra arquitetura. Combinar Page Object Model para interação com telas, Application Actions para jornadas de negócio e Functional Abstractions para comunicação HTTP, preparação de dados e funções reutilizáveis.

A combinação define responsabilidades; não exige criar todas as pastas em todo projeto. Em um projeto existente, respeitar as convenções locais e aplicar a separação de responsabilidades sem reorganizar arquivos fora do escopo solicitado.

### Responsabilidades e critérios de criação

| Camada | Responsabilidade | Quando criar |
| --- | --- | --- |
| `tests/` | Expressar o cenário, chamar operações e verificar os resultados esperados, preservando o ID e a origem do cenário. | Para os testes executáveis. |
| `pages/` | Encapsular seletores, leitura de estado e interações de uma tela ou componente. Receber o contexto de UI usado pelo teste. | Quando houver automação de UI. |
| `actions/` | Compor operações de pages e/ou services em uma jornada de negócio, com nome que expresse a intenção. | Quando uma jornada reutilizada ou suficientemente complexa se beneficiar dessa composição; também pode servir a fluxos somente de API. |
| `services/` | Encapsular operações HTTP/API e devolver respostas ou resultados necessários às verificações. Receber o contexto HTTP apropriado. | Quando houver chamadas API que precisem de uma interface reutilizável ou organização por domínio. |
| `helpers/` | Implementar transformações pequenas, builders de payload e outras funções auxiliares coesas. | Quando houver lógica auxiliar concreta a extrair. |
| `fixtures/` | Compor dependências e gerenciar preparação, isolamento e limpeza de recursos usados pelos testes. | Quando o projeto precisar compartilhar esse ciclo de vida ou fornecer dependências aos testes. |
| `data/` | Armazenar massas estáticas fictícias e versionáveis. | Quando dados separados do código forem necessários; não armazenar segredos ou dados pessoais reais. |

Os nomes acima são a convenção para novos projetos. Não criar classes-base, wrappers, arquivos vazios ou camadas que apenas repassem uma chamada sem benefício concreto.

### Direção das dependências

| Origem | Dependências permitidas no projeto |
| --- | --- |
| Tests | Fixtures, actions, pages, services, helpers e data conforme o cenário. |
| Fixtures | Actions, pages, services, helpers e data para montar o contexto e cuidar de recursos. |
| Actions | Pages, services, helpers e data para compor jornadas. |
| Pages | Helpers e APIs de UI; sem dependência de services, actions, fixtures ou tests. |
| Services | Helpers e APIs HTTP; sem dependência de pages, actions, fixtures ou tests. |
| Helpers | Outras funções auxiliares sem ciclos; sem importar camadas superiores. |
| Data | Sem importações de código de execução. |

As dependências do framework não são camadas internas do projeto. Fixtures fornecem contextos e instâncias; pages, services e actions recebem o que precisam por parâmetros ou construtores, sem importar a fixture que os criou. Actions compõem pages e services; uma page não deve chamar um service para concluir a jornada. Evitar ciclos entre módulos.

O teste pode chamar diretamente uma page ou um service quando a operação for simples. A passagem por actions não é obrigatória.

### Assertions e resultados

- Manter nos tests as verificações que determinam se o comportamento esperado do cenário foi atendido.
- Pages podem expor estado ou locators para essas verificações; não devem esconder a validação principal do cenário dentro de métodos de interação.
- Actions devolvem os resultados necessários ao teste e não decidem o aceite do cenário.
- Services preservam o acesso aos dados relevantes da resposta, inclusive erros esperados em testes negativos. Não ocultar esses resultados com validações genéricas de sucesso.
- Fixtures podem detectar falha na preparação ou limpeza; essa verificação de infraestrutura não substitui as assertions do cenário.
- Helpers podem calcular ou transformar valores para uma verificação, mantendo explícita no teste a expectativa de negócio.

### Aplicação por tipo de projeto

| Contexto | Composição sugerida | Exemplo de fluxo |
| --- | --- | --- |
| Somente API | Tests e services quando a abstração trouxer benefício; helpers, fixtures, data e actions conforme necessidade. Sem pages. | Teste chama operação de criação no service e verifica a resposta. Uma action pode compor uma jornada de várias operações. |
| Somente UI | Tests e pages; actions para jornadas compostas e demais camadas conforme necessidade. Sem services quando não houver chamadas API. | Teste usa uma page para preencher a tela e verifica o resultado observado; uma action pode coordenar várias telas. |
| UI com preparação por API | Tests, pages e services; fixtures podem preparar os dados e actions podem compor a jornada. | Fixture prepara um recurso por API, teste o utiliza pela UI e verifica o resultado; a limpeza remove apenas os recursos criados para o teste. |
| Fluxo misto de negócio | Tests, pages, services e actions quando a composição justificar. | Action combina operações de UI e API e devolve os resultados; teste verifica os critérios do cenário. |

Ao usar API para preparar um teste UI, não substituir a interação que o cenário pretende validar. Se o objetivo for validar criação pela interface, criar o objeto via API não cobre esse comportamento.

### Isolamento e entrega

- Evitar instâncias globais mutáveis de pages, clientes ou massa compartilhada entre testes. Definir o ciclo de vida e o isolamento conforme o recurso e as convenções do projeto.
- Manter cada teste independente da execução anterior; compartilhar a rotina de preparação, não depender do resultado de outro teste.
- Documentar as camadas efetivamente usadas e o motivo das abstrações relevantes. Não criar diretórios apenas para reproduzir a estrutura completa.
- Ao revisar a estrutura, conferir responsabilidades, ausência de dependências circulares e visibilidade das assertions principais no teste.
