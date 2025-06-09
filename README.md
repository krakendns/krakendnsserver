# krakendnsserver
krakendnsserver

🦑 KrakenDNS Server

DNS público, resiliente e privado

KrakenDNS é um projeto independente de resolução DNS que oferece **respostas rápidas**, **criptografia moderna** e **nenhum registro de atividades**. Ideal para usuários que desejam **privacidade**, **velocidade global** e proteção.

🌐 Endereços Globais

| Região        | IP                   | Provedor         |
|---------------|----------------------|------------------|
| 🇺🇸 Chicago    | `144.202.57.221`     | Vultr            |
| 🇺🇸 Nova York  | `162.243.238.171`    | DigitalOcean     |
| 🇩🇪 Frankfurt  | `5.22.215.185`       | UpCloud          |
| 🇯🇵 Tóquio     | `45.77.28.252`       | Vultr            |
| 🇫🇮 Finlândia  | `94.237.14.77`       | UpCloud          |
| 🇸🇬 Singapura  | `139.180.135.67`     | Vultr            |

## Por que escolher o Kraken DNS? 🦑
Kraken DNS é um serviço de DNS público gratuito projetado para desempenho excepcional:
**Latência ultra-baixa**: Otimizado garantindo respostas rápidas.
**Cache**: Acelera consultas frequentes com armazenamento eficiente.

**🛡️ Proteção Integrada**

Bloqueio de malware automático
Filtros anti-phishing atualizados
Proteção contra tracking opcional
Listas personalizadas de bloqueio

**Como usar no Linux**:

**Método Rápido (uma linha)**:

echo "nameserver 144.202.57.221" | sudo tee /etc/resolv.conf

**Método Permanente (systemd-resolved)**:

# Configurar DNS principal
sudo systemctl stop systemd-resolved
echo "DNS=144.202.57.221 162.243.238.171" | sudo tee -a /etc/systemd/network/dns.conf
sudo systemctl start systemd-resolved

# Verificar configuração
resolvectl status

**Testes**

# Testar resolução
dig @144.202.57.221 google.com

# Testar latência
ping -c 4 144.202.57.221

**Forma recomendada para Pi-hole (modo padrão, porta 53)**

sudo nano /etc/dnsmasq.d/99-kraken.conf

server=144.202.57.221#53
server=162.243.238.171#53
server=5.22.215.185#53
server=45.77.28.252#53
server=94.237.14.77#53
server=139.180.135.67#53

pihole restartdns


**Ubuntu/Debian**:


bash#!/bin/bash
# Salvar como install-kraken.sh
set -e

echo "🐙 Instalando KrakenDNS..."

# Backup da configuração atual
sudo cp /etc/resolv.conf /etc/resolv.conf.backup

# Configurar KrakenDNS
sudo tee /etc/resolv.conf << EOF
# KrakenDNS Configuration
nameserver 144.202.57.221
nameserver 162.243.238.171
nameserver 45.77.28.252
options timeout:2
options attempts:3
EOF

# Tornar imutável
sudo chattr +i /etc/resolv.conf

echo "✅ KrakenDNS instalado com sucesso!"
echo "🧪 Testando resolução..."
dig @144.202.57.221 google.com +short
CentOS/RHEL:
bash#!/bin/bash



**sistemas Red Hat**


systemctl stop NetworkManager
echo -e "DNS1=144.202.57.221\nDNS2=162.243.238.171" >> /etc/sysconfig/network-scripts/ifcfg-eth0
systemctl start NetworkManager


Por que o KrakenDNS **não utiliza DoH como padrão**

O protocolo **DNS-over-HTTPS (DoH)** é uma tecnologia importante para proteger a privacidade dos usuários contra interceptações, especialmente em redes públicas. Contudo, o KrakenDNS **optou tecnicamente por não utilizar DoH como padrão** neste momento por motivos **técnicos e de segurança**:

**Ataques TLS**: Certificados podem ser comprometidos
**Incompatibilidade**: Cloudflare Proxy quebra Android e IOS
**Exposição de serviços**: Painéis administrativos ficam acessíveis


![image](https://github.com/user-attachments/assets/f0291917-71a8-47bf-9f9c-65314c33f15c)


**Nossa solução**: Utilizamos DNSCrypt e Cloudflare poxy internamente para criptografia entre servidores, oferecendo segurança sem as vulnerabilidades web.

[Usuário] 
   ↓ (Consulta DNS)
[Kraken DNS: 144.202.57.221]
   ↓ (Segurança)
┌──────────────────────────────┐
│ 🔐 Proxy Cloudflare (DoH) │
│ ou DNSCrypt Local (127.0.0.1) │
└──────────────────────────────┘
   ↓ (Resolução)
[🌐 Domínio (Cache Otimizado)]

Mesmo sem DoH no lado do **usuário**, a **privacidade é garantida dentro do Kraken** com criptografia interna, upstreams seguros e filtragem baseada em reputação.

📌 Futuro do DoH no Kraken

O suporte ao DoH **está em desenvolvimento experimental**, mas será **ativado apenas quando a estabilidade, performance e privacidade puderem ser mantidas em alto nível**, sem comprometer a experiência dos usuários.

Utilizamos uma **infraestrutura** com múltiplos provedores de upstream, cuidadosamente escolhidos por seus **princípios éticos**, **desempenho** e **respeito à privacidade**.

**Por que escolher um DNS quando você pode ter o melhor de todos?**
O KrakenDNS é possível graças à excelência de outras iniciativas que inspiram confiança, segurança e inovação no universo do DNS. O KrakenDNS reconhece e agradece publicamente as iniciativas que tornam a internet mais rápida, segura e ética. São projetos que inspiram nossa missão e ajudam a elevar a qualidade da resolução DNS para todos:

🚀 [Cloudflare](cloudflare.com) — Por oferecer uma das infraestruturas mais rápidas e resilientes do mundo.

🛡️ [Quad9](quad9.net) — Por proteger os usuários contra ameaças reais, sem comprometer a privacidade.

⚙️ [ControlD](controld.com) — Pelos filtros personalizáveis avançados e compromisso com escolhas livres de rastreamento.

🔒 [Mullvad](mullvad.net) — Por provar que privacidade de verdade ainda existe — sem logs, sem IDs, sem firulas.

🚫 [Adguard](adguard.com) — Pelo bloqueio eficiente de anúncios, rastreadores e conteúdo nocivo.

👨‍👩‍👧‍👦 [CleanBrowsing](cleanbrowsing.org) — Por oferecer proteção confiável para famílias e ambientes educacionais.




**É fundamental garantir a transparência com os usuários e evitar qualquer promessa enganosa.**

Mesmo utilizando o painel de controle **[AdGuard Home](https://github.com/AdguardTeam/AdGuardHome)**, um software de código aberto, que possui a capacidade de registrar logs, O KrakenDNS não registra logs. Nosso serviço de DNS público é construído utilizando o AdGuard Home, uma plataforma para filtragem de DNS. Embora o AdGuard Home possua a funcionalidade de registro de logs, nossa configuração é estritamente definida para não armazenar quaisquer logs de consultas DNS. Implementamos as configurações necessárias para garantir que nenhuma informação sobre suas atividades de navegação seja persistentemente registrada em nossos servidores. Nosso foco é fornecer um serviço de DNS rápido e privado. O sistema do KrakenDNS mantém dados temporários para fins de desempenho, como tempo Médio de resposta upstream e melhores servidores DNS. Os diagnósticos não são persistentes ou identificáveis. Esses diagnósticos são voláteis e não vinculados a IPs ou identidades. Transparência e ética são pilares do KrakenDNS.

**Infraestrutura**

O KrakenDNS é hospedado em algumas das melhores instâncias de servidores virtuais do mercado, incluindo a DigitalOcean, Vultr e Upcloud, reconhecidas por sua estabilidade, baixa latência e flexibilidade de configuração. Estamos continuamente testando novas plataformas, mas muitas delas infelizmente impõem restrições no kernel do Linux, o que compromete a flexibilidade necessária para manter um serviço de DNS público avançado, transparente e seguro. Nosso projeto é 100% financiado com recursos próprios, sem anúncios, rastreadores ou patrocínio corporativo.

**Nossa missão e responsabilidade**

O KrakenDNS compreende que um serviço de DNS vai muito além da simples resolução técnica de nomes. Para muitas pessoas ao redor do mundo, é a porta de entrada para experiências básicas e essenciais na internet, seja acessar um filme, conectar-se com entes queridos ou buscar informações importantes. Reconhecemos a enorme responsabilidade que isso representa. Cada escolha técnica, cada servidor, cada configuração de segurança que implementamos é guiada por essa consciência: estamos proporcionando um serviço fundamental que impacta vidas. É com esse senso de propósito que selecionamos cuidadosamente nossos provedores de infraestrutura e refinamos constantemente nossos protocolos de segurança. A credibilidade que construímos não se mede apenas em latência ou uptime, mas na confiança que depositam em nós para acessar uma internet mais aberta e segura. Esta percepção eleva o KrakenDNS de um simples serviço técnico a uma iniciativa com impacto social significativo, uma responsabilidade que levamos muito a sério.

**Contribuições: Áreas que precisam de ajuda**

O KrakenDNS é um projeto independente, sem financiamento externo. Toda ajuda é bem-vinda desde que com responsabilidade e alinhada com nossa missão de proteger a liberdade digital.

Aqui estão áreas onde sua ajuda pode fazer a diferença:

Documentação em outros idiomas
Guias de instalação local para Android, iOS e roteadores
Instaladores rápidos (bash, .deb, .apk, .yaml)
Listas de bloqueio regionais baseadas em DNS
Regras de firewall inteligentes (iptables, nftables, dnsdist)
Scripts para mitigar DoS e DNSKEY flood
Relatórios de desempenho de novas regiões
Sugestões de provedores VPS confiáveis e compatíveis com kernel avançado.
Materiais educativos sobre privacidade e DNS seguro
Colaboração com comunidades técnicas e universidades

⚠️ Importante: o KrakenDNS não aceitará integrações com sistemas que envolvam rastreamento, coleta de dados pessoais, ou interesses corporativos que contradigam nossos princípios.

Abra uma issue ou envie uma sugestão explicando sua ideia. Vamos construir algo transparente, seguro e com impacto real!

**Política de Segurança e Uso Aceitável**

Informamos que não são permitidas atividades como varreduras de portas não autorizadas, ataques de negação de serviço (DDoS), envio de spam por meio de consultas DNS, ou qualquer outro comportamento que possa comprometer a integridade do nosso serviço. Essas ações são consideradas invasivas e podem afetar a segurança da nossa infraestrutura. Além disso, qualquer endereço IP que seja identificado como envolvido em atividades de spam ou outras ações prejudiciais ao nosso serviço será banido imediatamente. Nossa prioridade é garantir um ambiente seguro e confiável para todos os usuários, e tomaremos as medidas necessárias para proteger nossa rede contra abuso. Todos os acessos são monitorados por sistemas de segurança que identificam e bloqueiam automaticamente comportamentos suspeitos. Endereços IP envolvidos em abusos serão banidos.

🧠 **Reconhecimento a profissionais e pesquisadores de segurança**

Reconhecemos a importância do trabalho de profissionais e pesquisadores de segurança na melhoria da segurança online. A pesquisa legítima de segurança, que visa identificar vulnerabilidades e fortalecer a segurança da infraestrutura da internet, é fundamental para o avanço da segurança cibernética. Estamos dispostos a colaborar com iniciativas que promovam a segurança cibernética, desde que atendam a dois critérios.

1 - A pesquisa seja realizada de forma ética e controlada, garantindo que não haja danos ou riscos desnecessários.

2 - A pesquisa não afete a disponibilidade do serviço para nossos usuários, garantindo que eles possam continuar a utilizar nossos recursos sem interrupções.
Nossa abordagem busca equilibrar a proteção do nosso serviço com o apoio ao avanço da segurança na internet como um todo, reconhecendo a importância da colaboração e do compartilhamento de conhecimentos para criar um ambiente online mais seguro.

Nosso compromisso com a privacidade e liberdade dos usuários exige um esforço constante para evitar abusos. Ao utilizar o KrakenDNS, você concorda em respeitar os limites de uso ético e contribuir para um ecossistema DNS confiável.

**Pesquisadores responsáveis** são bem-vindos e podem entrar em contato diretamente conosco.

krakendnsserver@protonmail.com
