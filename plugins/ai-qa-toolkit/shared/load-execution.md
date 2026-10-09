# Execução de carga

Fonte única da regra que separa gerar scripts de desempenho de executar carga.

Gerar scripts e executar carga são ações distintas. Executar carga somente quando isso estiver solicitado ou autorizado, com alvo, perfil, limites e condições de parada definidos. Aproveitar autorizações já dadas dentro desse escopo.

- **Quem define:** alvo, perfil, limites e condições de parada são definidos ou confirmados pelo usuário. Se o pedido de execução não os trouxer, não execute: proponha valores e peça confirmação. Um pedido como "rode um teste rápido contra X" informa o alvo, mas não autoriza o agente a escolher sozinho o perfil, os limites e a parada.
- **Alvo de terceiros:** se o alvo não for comprovadamente do usuário (domínio público, de exemplo ou de terceiros), confirme que o usuário tem autorização para testá-lo antes de qualquer requisição.
- Verificações que enviem requisições, mesmo curtas, também devem respeitar esse alvo, perfil e escopo.
- Verificações locais de configuração não comprovam desempenho.
- Registrar o que foi executado.
- Não usar endpoints públicos de exemplo como destino automático.
