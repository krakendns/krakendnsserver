## 🚨 Eventos Documentados

**Evento #001: Grande Degradação dos Repositórios Fedora 13/06/2025**

📅 Data: 13-14 de Junho de 2025

⏰ Início: 13/06/2025 às 22:00 UTC

🌍 Escopo: Global

Detectada degradação severa de performance no acesso aos repositórios Fedora/EPEL em múltiplos provedores de cloud globalmente. Downloads que normalmente levam segundos passaram a demorar 15+ minutos.
Infraestrutura Afetada.

**Provedores de Cloud**: DigitalOcean, Vultr, Linode, Contabo


**Regiões** América do Norte, Europa, Ásia e Oceania


**Timeline de Detecção**

13/06/2025 22:00: Primeira detecção durante atualização

13/06/2025 23:00: Confirmação do problema em múltiplos providers

14/06/2025 14:00: Mapeamento geográfico iniciado

14/06/2025 16:00: DigitalOcean parcialmente recuperado

14/06/2025 18:00: Contabo afetada 


**Metodologia de Investigação**

Detecção Inicial: Timeout durante instalação de pacotes

Confirmação Multi-Provider: Testes na DigitalOcean, Vultr, Linode

Mapeamento Geográfico: Deploy de VPS em múltiplos continentes

Análise de Padrões: Identificação de edge locations específicos


| Localização    | Provider       | Status       | Observações            |
|----------------|----------------|--------------|-------------------------|
| Amsterdã       | DigitalOcean   | ❌ ✅         | Recuperou parcialmente  |
| Frankfurt      | DigitalOcean   | ✅            | Recuperou       |
| NYC            | DigitalOcean   | ✅            | Recuperou       |
| Sydney         | Contabo        | ❌            | Afetado posteriormente  |
| Alemanha       | Contabo        | ❌            | Afetado posteriormente  |
| Vultur Global  | Vultur         | ❌            | Ainda afetado           |
| Linode Global  | Linode         | ❌            | Ainda afetado           |


**🤝 Contribuições**
Administradores de infraestrutura que observaram comportamentos similares são encorajados a compartilhar suas observações para melhorar nossa compreensão destes eventos.

**⚠️ Disclaimer**
As informações aqui documentadas são baseadas em observações reais de infraestrutura, mas as hipóteses técnicas são especulativas até confirmação oficial dos provedores afetados. Este documento serve como registro histórico e ferramenta de análise para a comunidade técnica.
