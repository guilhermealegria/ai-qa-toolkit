# Objetivo

Definir padrões arquiteturais, convenções e boas práticas para automação de testes utilizando Playwright.

Este documento deve ser utilizado como referência principal para geração automatizada de código via IA.

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