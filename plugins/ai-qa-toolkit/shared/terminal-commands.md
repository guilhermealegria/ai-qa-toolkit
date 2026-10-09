# Comandos no terminal

Fonte única da regra sobre shell, válida para qualquer agente.

- Antes de executar comandos, identificar o shell ativo informado pelo ambiente, pela IDE ou pelo contexto da sessão. Não assumir Bash, Linux ou WSL por padrão.
- Adaptar a sintaxe ao shell ativo. Em PowerShell, usar cmdlets (`Get-ChildItem`, `Get-Content`, `Select-String`, `Set-Location`, `$env:VAR = "valor"`) e `Remove-Item` com cuidado explícito; evitar construções exclusivas de Bash, como `&&`, `||`, `export`, `source`, `grep`, `sed`, `awk` e redirecionamentos específicos.
- Em caso de dúvida sobre o shell, verificar com um comando compatível ou perguntar antes de executar comandos dependentes dele.
- Os comandos entregues ao usuário devem corresponder ao shell e ao projeto de destino.
