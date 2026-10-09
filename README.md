# AI QA Toolkit

Plugin de skills de QA para o **Claude Code** e o **Codex**. Ajuda a analisar requisitos, escrever cenários funcionais, automatizar com Playwright, planejar desempenho e implementar scripts k6, com regras explícitas de escopo e autorização.

O toolkit não é um projeto de automação: ele orienta o agente. O código gerado vai para um projeto **fora** do toolkit.

## O que o plugin traz

Um plugin, `ai-qa-toolkit`, com seis skills:

| Skill | Use para | Entrega |
| --- | --- | --- |
| `qa-flow` | Enviar arquivos sem dizer o que fazer, pedir mais de uma etapa ou decidir a próxima. | Pergunta o que falta e encaminha para a skill certa. |
| `qa-analysis` | Analisar user stories, critérios de aceite, contratos de API, logs e regras de negócio. | Fatos, gaps, riscos, edge cases, testabilidade e perguntas para o time. |
| `qa-scenarios` | Escrever cenários funcionais e, se pedido, a exportação para o Xray. | Cenários em BDD ou step-by-step e CSV de importação do Xray. |
| `qa-playwright` | Automatizar cenários de API e UI com Playwright. | Código de teste em JavaScript (ou a linguagem pedida), em projeto externo. |
| `qa-nft-plan` | Levantar requisitos e escrever cenários de desempenho, ou interpretar resultados. | Matriz de requisitos e fichas de cenário, independentes de ferramenta. |
| `qa-k6` | Implementar cenários de desempenho já planejados em k6. | Scripts k6 e registro do que foi verificado. |

## Regras que o plugin aplica

- **Análise e cenários não implicam automação.** A passagem para código exige pedido ou autorização, e aprovar cenários, por si só, não autoriza implementar.
- **Gerar scripts não executa carga.** Executar carga exige alvo, perfil, limites e condições de parada definidos ou confirmados por você. Alvos de terceiros exigem confirmação de autorização.
- **Projetos de automação ficam fora do toolkit.** Sem destino informado, o agente pergunta onde criar.
- **Nada é inventado.** Informação ausente vira gap, risco ou premissa; metas de desempenho desconhecidas ficam como `A definir`.
- **Segurança é só risco de QA.** A análise lista riscos e edge cases, mas não substitui revisão de segurança especializada.

## Instalação

Requisitos: Claude Code ou Codex CLI instalado. Versões em que o plugin foi testado: Claude Code 2.1.290, 2.1.292 e 2.1.293; Codex CLI 0.155.1 e 0.160.1. Outras versões podem funcionar, mas não foram verificadas.

### Claude Code

```bash
claude plugin marketplace add guilhermealegria/ai-qa-toolkit
claude plugin install ai-qa-toolkit@ai-qa-toolkit-marketplace
```

Também é possível, dentro de uma sessão, usar `/plugin` para instalar. Para usar um clone local em vez do GitHub, troque o primeiro comando por `claude plugin marketplace add <caminho-do-clone>`.

### Codex

```bash
codex plugin marketplace add guilhermealegria/ai-qa-toolkit
codex plugin add ai-qa-toolkit@ai-qa-toolkit-marketplace
```

Para um clone local: `codex plugin marketplace add <caminho-do-clone>`.

### Instalar uma versão específica

Cada versão publicada tem uma tag no formato `ai-qa-toolkit--v<versão>`. Para fixar a instalação em uma delas (veja as tags do repositório para as versões disponíveis), por exemplo a `0.1.0`:

```bash
claude plugin marketplace add guilhermealegria/ai-qa-toolkit@ai-qa-toolkit--v0.1.0
claude plugin install ai-qa-toolkit@ai-qa-toolkit-marketplace

codex plugin marketplace add guilhermealegria/ai-qa-toolkit --ref ai-qa-toolkit--v0.1.0
codex plugin add ai-qa-toolkit@ai-qa-toolkit-marketplace
```

### Conferir a instalação

```bash
claude plugin list
codex plugin list
```

O plugin deve aparecer como habilitado, na versão `0.1.1` (ou na mais recente publicada).

## Uso

Peça em linguagem natural; as skills são ativadas pela descrição. Também é possível citar a skill pelo nome.

- **Claude Code:** `/ai-qa-toolkit:qa-flow` ou, no texto do pedido, "use a skill qa-scenarios".
- **Codex:** cite a skill no pedido, por exemplo "usando a skill qa-scenarios, escreva os cenários desta história".

Exemplos:

```text
Analise esta user story e aponte gaps e riscos. <história>
Escreva os cenários BDD desta história e gere o CSV do Xray.
Automatize estes cenários aprovados em Playwright no projeto <caminho-externo>.
Quero levantar requisitos de desempenho para a busca do catálogo.
Implemente em k6 a ficha PERF-001 aprovada no projeto <caminho-externo>.
```

Fluxo típico: `qa-analysis` → `qa-scenarios` → `qa-playwright`, ou `qa-nft-plan` → `qa-k6`. Cada etapa pode ser pedida isoladamente. Com um pedido misto, o `qa-flow` aplica as skills na ordem.

### Onde ficam os arquivos

- **Inputs** (documentação, contratos, logs): de preferência em `inputs/` do seu workspace, ou no caminho que você informar. O conteúdo real de `inputs/` não deve ser versionado.
- **Artefatos de QA** (cenários, fichas, exportações): no destino que você informar. Se o workspace for o checkout deste repositório, a convenção é `generated-scenarios/`, já ignorada pelo Git.
- **Projetos de automação**: sempre fora do toolkit e da pasta de instalação do plugin.

## Atualização

A versão está fixada nos manifestos. Quem já instalou só recebe mudanças quando a versão sobe.

### Claude Code

```bash
claude plugin marketplace update ai-qa-toolkit-marketplace
claude plugin update ai-qa-toolkit@ai-qa-toolkit-marketplace
```

O próprio comando avisa que é preciso reiniciar a sessão para aplicar. Testado com um marketplace local (0.1.0 para 0.1.1).

### Codex

Repita a instalação; o Codex substitui a cópia em cache pela versão atual do marketplace:

```bash
codex plugin add ai-qa-toolkit@ai-qa-toolkit-marketplace
```

Se o marketplace foi adicionado pelo GitHub, atualize antes o clone dele com `codex plugin marketplace upgrade ai-qa-toolkit-marketplace`. Essa etapa não foi testada: o `upgrade` só aceita marketplaces Git e o teste de atualização foi feito com um marketplace local.

## Desinstalação

```bash
claude plugin uninstall ai-qa-toolkit@ai-qa-toolkit-marketplace
claude plugin marketplace remove ai-qa-toolkit-marketplace

codex plugin remove ai-qa-toolkit@ai-qa-toolkit-marketplace
codex plugin marketplace remove ai-qa-toolkit-marketplace
```

## O que foi validado e limitações

O plugin foi validado com sessões reais nos dois agentes: ativação por descrição, leitura das regras compartilhadas, encaminhamento do `qa-flow`, escopo e autorização, comportamento de cada skill e instalação a partir do GitHub.

Limitações conhecidas:

- Os testes de implementação verificaram a decisão e o texto proposto, não a gravação dos arquivos nem a execução do código gerado.
- Na maioria dos casos houve uma execução por teste; o comportamento pode variar entre execuções.
- Não foram testados o aplicativo desktop, o Windows, marketplaces por HTTPS que não sejam o GitHub e a atualização de marketplaces Git no Codex.
- O plugin custa cerca de 761 tokens por sessão enquanto está habilitado, e de cerca de 2 mil a 4 mil tokens a cada skill acionada (estimativa do `claude plugin details`).

## Para quem mantém o repositório

### Estrutura

```text
.claude-plugin/marketplace.json     marketplace do Claude Code
.agents/plugins/marketplace.json    marketplace do Codex
plugins/ai-qa-toolkit/              o plugin: tudo o que é distribuído
├── .claude-plugin/plugin.json      manifesto do Claude Code
├── .codex-plugin/plugin.json       manifesto do Codex
├── LICENSE                         cópia da licença MIT, para acompanhar o pacote
├── shared/                         regras transversais, fonte única
└── skills/                         as seis skills
workflows/  inputs/  examples/  AGENTS.md  CLAUDE.md   manutenção, fora do pacote
```

O pacote é exatamente o conteúdo de `plugins/ai-qa-toolkit/` (22 arquivos). Em instalações reais a partir do GitHub, o cache de cada agente continha exatamente esses arquivos (o Claude Code acrescenta apenas um marcador interno `.in_use`).

### Regras de edição

- Edite as regras transversais **somente** em `plugins/ai-qa-toolkit/shared/`; as skills as referenciam por `../../shared/<arquivo>.md`. Não copie essas regras para dentro das skills.
- O `LICENSE` da raiz e o de `plugins/ai-qa-toolkit/` devem ser idênticos (`cmp LICENSE plugins/ai-qa-toolkit/LICENSE`): a cópia dentro do plugin é a que acompanha o pacote instalado.
- Cada skill deve funcionar só com o que está dentro de `plugins/ai-qa-toolkit/`. Não referencie arquivos de fora dessa pasta.
- Ao mudar uma descrição de skill, repita os testes de ativação (positivos e negativos).
- Os agentes instalam uma cópia em cache: depois de editar localmente, atualize (veja a seção Atualização) antes de testar.

### Verificações antes de publicar

```bash
claude plugin validate --strict plugins/ai-qa-toolkit
claude plugin validate --strict .
claude plugin details ai-qa-toolkit@ai-qa-toolkit-marketplace    # custo de contexto, com o plugin instalado
```

O Codex não tem comando de validação: confira os JSON de `.codex-plugin/plugin.json` e de `.agents/plugins/marketplace.json` e instale a partir do marketplace local (`codex plugin marketplace add <caminho-do-clone>` e `codex plugin add ai-qa-toolkit@ai-qa-toolkit-marketplace`).

### Lançamento de uma versão

1. Suba o campo `version` em `plugins/ai-qa-toolkit/.claude-plugin/plugin.json` e em `plugins/ai-qa-toolkit/.codex-plugin/plugin.json`, mantendo os dois iguais.
2. Commit e push da branch.
3. Crie a tag da versão: `claude plugin tag plugins/ai-qa-toolkit` (formato `ai-qa-toolkit--v<versão>`; use `--dry-run` antes). O comando recusa tags com alterações não commitadas.
4. Teste a instalação e a atualização como descrito acima.

### Testes

As notas de acompanhamento ficam na pasta local `docs/`, que não é versionada. Para repetir a validação depois de uma mudança, instale o plugin nos dois agentes e confira, em sessões novas e a partir de uma pasta fora do toolkit:

- **Ativação:** cada pedido típico ativa a skill certa (análise, cenários, Playwright, desempenho, k6) e pedidos sem relação com QA não ativam nenhuma.
- **Leitura das regras compartilhadas:** uma pergunta que dependa de `shared/` (por exemplo, as colunas do step-by-step) é respondida com leitura do arquivo no cache do agente.
- **Autorização:** "gere apenas os cenários" não inicia automação; cenários aprovados, por si só, não autorizam implementar; "não automatize ainda" é respeitado.
- **Carga:** pedir para executar um teste sem alvo, perfil, limites e condições de parada não executa nada e pede confirmação.
- **Destino:** pedir arquivos sem informar onde criá-los faz o agente perguntar o destino, em vez de gravar no diretório atual.
- **Intenção:** enviar só arquivos, ou uma mensagem vazia após "My request for Code:", faz o `qa-flow` perguntar a tarefa, sem ler o conteúdo antes.

## Licença

[MIT](LICENSE).
