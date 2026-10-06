# Linguagem e ecossistema

A linguagem deve ser definida com base na instrução do usuário:

- Usar JavaScript por padrão. Usar TypeScript ou outra linguagem suportada somente quando explicitamente solicitado.
- Se o projeto indicado usar outra linguagem e não houver escolha explícita, esclarecer a divergência antes de adicionar código incompatível; não migrar o projeto automaticamente.
- Se a linguagem solicitada não tiver suporte oficial, informar as alternativas suportadas e esclarecer a escolha, sem substituir silenciosamente a linguagem.
- Adaptar APIs, runner, dependências, configuração, convenções de arquivos e comandos à linguagem escolhida e ao projeto existente. A arquitetura define responsabilidades; os nomes e mecanismos devem seguir o ecossistema utilizado.

| Linguagem/ecossistema | Orientação para implementação |
| --- | --- |
| JavaScript ou TypeScript / Node.js | Usar Playwright Test por padrão, com `test()`, `test.describe` e `expect`. Respeitar o gerenciador de pacotes e os scripts do projeto. |
| Python | Usar o runner existente; para novos projetos, o plugin Pytest é a opção recomendada na documentação. Escolher a API síncrona ou assíncrona conforme o projeto. |
| Java | Respeitar o framework de testes e build existentes, como JUnit ou TestNG e Maven ou Gradle. |
| C# / .NET | Respeitar o framework de testes e as integrações Playwright correspondentes, como NUnit, MSTest ou xUnit. |

Conferir o suporte e a sintaxe na [documentação oficial de linguagens](https://playwright.dev/docs/languages) e na documentação da versão utilizada. Referências a `test()`, `request` e `await expect(...)` nesta skill descrevem o fluxo Node.js; usar os equivalentes da linguagem escolhida.
