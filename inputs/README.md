# Inputs

Use esta pasta para armazenar temporariamente arquivos de entrada enviados para análise, geração de cenários ou apoio à automação.

Exemplos:

```text
inputs/user-story-login.md
inputs/swagger-pedidos.yaml
inputs/log-erro-checkout.txt
```

Regras:

- Prefira esta pasta para inputs médios ou grandes, pois permite leitura seletiva e menor uso de tokens.
- Não versionar arquivos reais de input por padrão.
- Não colocar aqui projetos Playwright gerados.
- Não criar aqui pastas de automação como `tests/`, `pages/`, `actions/`, `services/`, `fixtures/`, `helpers/` ou `data/`.
