# Execução de carga

Fonte única da regra que separa gerar scripts de desempenho de executar carga.

Gerar scripts e executar carga são ações distintas. Executar carga somente quando isso estiver solicitado ou autorizado, com alvo, perfil, limites e condições de parada definidos. Aproveitar autorizações já dadas dentro desse escopo.

- Verificações que enviem requisições, mesmo curtas, também devem respeitar esse alvo e escopo.
- Verificações locais de configuração não comprovam desempenho.
- Registrar o que foi executado.
- Não usar endpoints públicos de exemplo como destino automático.
