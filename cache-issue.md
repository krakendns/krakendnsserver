## Experiência real: O impacto do cache local no bloqueio DNS

Durante o monitoramento do KrakenDNS, identificamos uma situação onde **consultas DNS maliciosas estavam sendo resolvidas normalmente**, mesmo com políticas de bloqueio ativas nos resolvers externos (AdGuard DNS, ControlD, etc).

## Causa identificada Cache vs Bloqueio:

**O cache local do AdGuard Home** utilizava entradas desatualizadas para responder consultas, sem consultar resolvers upstream, causando falhas nos bloqueios.

## Fluxo QUEBRADO

Cliente → KrakenDNS Cache → Resposta Direta  (Pula AdGuard/ControlD)

## Fluxo CORRETO

Cliente → KrakenDNS → AdGuard/ControlD → Filtros → Resposta

## Desabilitar Cache ##

desabilitar o cache local é a melhor opção.

**Nota:** Mesmo soluções de privacidade como o AdGuard Home podem criar brechas se mal configuradas. O uso de cache DNS deve ser alinhado com a estratégia de filtragem upstream para evitar inconsistências.

## Lições aprendidas

- Cache local melhora desempenho, mas pode comprometer segurança.

- Filtragem real exige trânsito pelos resolvers externos.
  
- Transparência operacional fortalece a confiança no KrakenDNS.




