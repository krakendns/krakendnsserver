# krakendnsserver


🦑 KrakenDNS Server

DNS público, resiliente e privado

KrakenDNS é um projeto independente de resolução DNS que oferece **respostas rápidas**, **criptografia moderna** e **nenhum registro de atividades**. Ideal para usuários que desejam **privacidade**, **velocidade global** e proteção.




## 🌐 Endereços Globais – KrakenDNS

| Região         | IP                | Provedor      |
|----------------|-------------------|---------------|
| ![US](https://flagcdn.com/24x18/us.png) New York   | `162.243.238.171` | Vultr         |
| ![DE](https://flagcdn.com/24x18/de.png) Frankfurt  | `199.247.21.0`    | Vultr         |
| ![JP](https://flagcdn.com/24x18/jp.png) Tokyo      | `45.77.28.252`    | Vultr         |
| ![SE](https://flagcdn.com/24x18/se.png) Sweden     | `172.234.100.170` | Linode        |
| ![SG](https://flagcdn.com/24x18/sg.png) Singapore  | `139.180.135.67`  | Vultr         |
| ![NL](https://flagcdn.com/24x18/nl.png) Amsterdam  | `95.179.151.156`  | Vultr         |
| ![FI](https://flagcdn.com/24x18/fi.png) Finland    | `5.61.88.206`     | Tietokettu    |
| ![AU](https://flagcdn.com/24x18/au.png) Australia  | `46.250.240.138`  | Contabo       |

## Status

[![](https://img.shields.io/badge/KrakenDNS-Status%3A%20Online-brightgreen)](https://stats.uptimerobot.com/eAyelEtTEt)




## Por que escolher o Kraken DNS? 🦑
Kraken DNS é um serviço de DNS público gratuito projetado para desempenho excepcional:

-**Latência ultra-baixa**: Otimizado garantindo respostas rápidas.

-**Cache**: Acelera consultas frequentes.

**🛡️ Proteção Integrada**

Bloqueio de malware automático
Filtros anti-phishing atualizados
Proteção contra tracking opcional
Listas personalizadas de bloqueio

## Como usar no Linux:

**Método Rápido (uma linha)**:

echo "nameserver 95.179.151.156" | sudo tee /etc/resolv.conf

**Método Permanente (systemd-resolved)**:

**Configurar DNS principal**:

sudo systemctl stop systemd-resolved
echo "DNS=95.179.151.156 162.243.238.171" | sudo tee -a /etc/systemd/network/dns.conf
sudo systemctl start systemd-resolved

**Verificar configuração**

resolvectl status

**Testar resolução**

dig @95.179.151.156 google.com

**Testar latência**

ping -c 4 95.179.151.156

**Backup da configuração atual**

sudo cp /etc/resolv.conf /etc/resolv.conf.backup

**Configurar KrakenDNS**
sudo tee /etc/resolv.conf << EOF
KrakenDNS Configuration
nameserver 95.179.151.156
nameserver 162.243.238.171
nameserver 45.77.28.252
options timeout:2
options attempts:3
EOF

**Tornar imutável**
sudo chattr +i /etc/resolv.conf

**sistemas Red Hat**

systemctl stop NetworkManager
echo -e "DNS1=95.179.151.156\nDNS2=162.243.238.171" >> /etc/sysconfig/network-scripts/ifcfg-eth0
systemctl start NetworkManager

## Forma recomendada para Pi-hole (modo padrão, porta 53)

sudo nano /etc/dnsmasq.d/99-kraken.conf

server=95.179.151.156#53
server=162.243.238.171#53
server=167.172.160.4#53
server=45.77.28.252#53
server=139.180.135.67#53

pihole restartdns

## Como usar no Windows:

![image](https://github.com/user-attachments/assets/c9a16890-8a3d-4893-a8ea-581a83931ccd)


## Mikrotik:


![mikrotik](https://github.com/user-attachments/assets/e2bcbda8-b74e-4eeb-94dc-94c5ef3ea3a3)

![mikrotik 2](https://github.com/user-attachments/assets/346f943f-2920-41a7-bd1b-b090695010a0)

![mikrotik3](https://github.com/user-attachments/assets/17f8a00b-4e77-454b-83ca-faf3f3a22993)

![mikrotik 4](https://github.com/user-attachments/assets/b1a20944-8f7c-41f4-83c6-1c6031f0c956)

![mikrotik 5](https://github.com/user-attachments/assets/43b09db2-5882-4fee-b6d2-6f6d9f79ef0e)

![mikrotik 6](https://github.com/user-attachments/assets/c5be9c38-76e3-4811-be00-c7346e0e671b)

![mikrotik 7](https://github.com/user-attachments/assets/128ea026-7e48-44c5-8af5-c4d826880ba7)


## TP-Link Huawei:

![huawei](https://github.com/user-attachments/assets/3d87517c-a261-46a9-8467-3176d77817bb)

![tplink-1](https://github.com/user-attachments/assets/3aac3ba1-8a35-4113-8db8-0bef8cc8e010)



## Por que o KrakenDNS **não utiliza DoH como padrão**

O protocolo **DNS-over-HTTPS (DoH)** é uma tecnologia importante para proteger a privacidade dos usuários contra interceptações, especialmente em redes públicas. Contudo, o KrakenDNS **optou tecnicamente por não utilizar DoH como padrão** neste momento por motivos **técnicos e de segurança**:

-**Ataques TLS**: Certificados podem ser comprometidos

-**Incompatibilidade**: Cloudflare Proxy quebra Android e IOS

-**Exposição de serviços**: Painéis administrativos ficam acessíveis


![image](https://github.com/user-attachments/assets/f0291917-71a8-47bf-9f9c-65314c33f15c)


## Nossa solução

O KrakenDNS implementa a **criptografia interna** Utilizamos DNSCrypt e Cloudflare poxy internamente para criptografia entre servidores, oferecendo segurança sem as vulnerabilidades web.

-**DNSCrypt Local (127.0.0.1)** para encriptação dentro do servidor

-**Proxy Cloudflare (127.0.0.1)** para ofuscar e proteger o IP real

-**Upstreams criptografados** 


### 🔁 Fluxo de uma consulta no KrakenDNS

**[Usuário]** ─►

─► **(Consulta DNS)**

─► **[KrakenDNS IP: 144.202.57.221]**

**[Camada de Segurança]**

─►1- DNSCrypt Local (127.0.0.1) 

─►2- Cloudflare Proxy Interno (127.0.0.1)

Mesmo sem DoH no lado do usuário, a privacidade é garantida dentro do Kraken com criptografia interna, upstreams seguros e filtragem baseada em reputação.


## Futuro do DoH no Kraken

O suporte ao DoH **está em desenvolvimento experimental**, mas será **ativado apenas quando a estabilidade, performance e privacidade puderem ser mantidas em alto nível**, sem comprometer a experiência dos usuários.

Utilizamos uma **infraestrutura** com múltiplos provedores de upstream, cuidadosamente escolhidos por seus **princípios éticos**, **desempenho** e **respeito à privacidade**.

**Por que escolher um DNS quando você pode ter o melhor de todos?**
O KrakenDNS é possível graças à excelência de outras iniciativas que inspiram confiança, segurança e inovação no universo do DNS. O KrakenDNS reconhece e agradece publicamente as iniciativas que tornam a internet mais rápida, segura e ética. São projetos que inspiram nossa missão e ajudam a elevar a qualidade da resolução DNS para todos:


🚀 [Cloudflare](https://www.cloudflare.com/pt-br/) — Por oferecer uma das infraestruturas mais rápidas e resilientes do mundo.  
🛡️ [Quad9](https://quad9.net/pt/) — Por proteger os usuários contra ameaças reais, sem comprometer a privacidade.  
⚙️ [ControlD](https://controld.com/) — Pelos filtros personalizáveis avançados e compromisso com escolhas livres de rastreamento.  
🔒 [Mullvad](https://mullvad.net/pt) — Por provar que privacidade de verdade ainda existe.  
🚫 [AdGuard](https://adguard.com) — Pelo bloqueio de anúncios e tracking.  
👨‍👩‍👧‍👦 [CleanBrowsing](https://cleanbrowsing.org/) — Por oferecer proteção familiar de qualidade.





## É fundamental garantir a transparência com os usuários e evitar qualquer promessa enganosa.

Mesmo utilizando o painel de controle **[AdGuard Home](https://github.com/AdguardTeam/AdGuardHome)**, um software de código aberto, que possui a capacidade de registrar logs, O KrakenDNS não registra logs. Nosso serviço de DNS público é construído utilizando o AdGuard Home, uma plataforma para filtragem de DNS. Embora o AdGuard Home possua a funcionalidade de registro de logs, nossa configuração é estritamente definida para não armazenar quaisquer logs de consultas DNS. Implementamos as configurações necessárias para garantir que nenhuma informação sobre suas atividades de navegação seja persistentemente registrada em nossos servidores. Nosso foco é fornecer um serviço de DNS rápido e privado. O sistema do KrakenDNS mantém dados temporários para fins de desempenho, como tempo Médio de resposta upstream e melhores servidores DNS. Os diagnósticos não são persistentes ou identificáveis. Esses diagnósticos são voláteis e não vinculados a IPs ou identidades. Transparência e ética são pilares do KrakenDNS.

## Infraestrutura

O KrakenDNS é hospedado em algumas das melhores instâncias de servidores virtuais do mercado, incluindo a DigitalOcean, Vultr e Linode, reconhecidas por sua estabilidade, baixa latência e flexibilidade de configuração. Estamos continuamente testando novas plataformas, mas muitas delas infelizmente impõem restrições no kernel do Linux, o que compromete a flexibilidade necessária para manter um serviço de DNS público avançado, transparente e seguro. Nosso projeto é 100% financiado com recursos próprios, sem anúncios, rastreadores ou patrocínio corporativo.

## Nossa missão e responsabilidade

O KrakenDNS compreende que um serviço de DNS vai muito além da simples resolução técnica de nomes. Para muitas pessoas ao redor do mundo, é a porta de entrada para experiências básicas e essenciais na internet, seja assistir a um filme, conectar-se com seus familiares e amigos ou buscar informações importantes. Reconhecemos a enorme responsabilidade que isso representa. Cada escolha técnica, cada servidor, cada configuração de segurança que implementamos é guiada por essa consciência: estamos proporcionando um serviço fundamental que impacta vidas. É com esse senso de propósito que selecionamos cuidadosamente nossos provedores de infraestrutura e refinamos constantemente nossos protocolos de segurança. A credibilidade que construímos não se mede apenas em latência ou uptime, mas na confiança que depositam em nós para acessar uma internet mais aberta e segura. Esta percepção eleva o KrakenDNS de um simples serviço técnico a uma iniciativa com impacto social significativo, uma responsabilidade que levamos muito a sério.

## Contribuições: Áreas que precisam de ajuda

O KrakenDNS é um projeto independente, sem financiamento externo. Toda ajuda é bem-vinda desde que com responsabilidade e alinhada com nossa missão de proteger a liberdade digital.

Aqui estão áreas onde sua ajuda pode fazer a diferença:

1 - Documentação em outros idiomas

2 - Guias de instalação local para Android, iOS e roteadores

3 - Instaladores rápidos (bash, .deb, .apk, .yaml)

4 - Listas de bloqueio regionais baseadas em DNS

5 - Regras de firewall inteligentes

6 - Scripts para mitigar DoS e DNSKEY flood

7 - Relatórios de desempenho de novas regiões

8 - Sugestões de provedores VPS confiáveis e compatíveis com kernel avançado.

9 - Materiais educativos sobre privacidade e DNS seguro

10- Colaboração com comunidades técnicas e universidades

⚠️ Importante: o KrakenDNS não aceitará integrações com sistemas que envolvam rastreamento, coleta de dados pessoais, ou interesses corporativos que contradigam nossos princípios.

Abra uma issue ou envie uma sugestão explicando sua ideia. Vamos construir algo transparente, seguro e com impacto real!


## Política de Segurança e Uso Aceitável

Informamos que não são permitidas atividades como varreduras de portas não autorizadas, ataques de negação de serviço (DDoS), envio de spam por meio de consultas DNS, ou qualquer outro comportamento que possa comprometer a integridade do nosso serviço. Essas ações são consideradas invasivas e podem afetar a segurança da nossa infraestrutura. Além disso, qualquer endereço IP que seja identificado como envolvido em atividades de spam ou outras ações prejudiciais ao nosso serviço será banido imediatamente. Nossa prioridade é garantir um ambiente seguro e confiável para todos os usuários, e tomaremos as medidas necessárias para proteger nossa rede contra abuso. Todos os acessos são monitorados por sistemas de segurança que identificam e bloqueiam automaticamente comportamentos suspeitos. Endereços IP envolvidos em abusos serão banidos.

## Reconhecimento a profissionais e pesquisadores de segurança🧠

Reconhecemos a importância do trabalho de profissionais e pesquisadores de segurança na melhoria da segurança online. A pesquisa legítima de segurança, que visa identificar vulnerabilidades e fortalecer a segurança da infraestrutura da internet, é fundamental para o avanço da segurança cibernética. Estamos dispostos a colaborar com iniciativas que promovam a segurança cibernética, desde que atendam a dois critérios.

1 - A pesquisa seja realizada de forma ética e controlada, garantindo que não haja danos ou riscos desnecessários.

2 - A pesquisa não afete a disponibilidade do serviço para nossos usuários, garantindo que eles possam continuar a utilizar nossos recursos sem interrupções.
Nossa abordagem busca equilibrar a proteção do nosso serviço com o apoio ao avanço da segurança na internet como um todo, reconhecendo a importância da colaboração e do compartilhamento de conhecimentos para criar um ambiente online mais seguro.

Nosso compromisso com a privacidade e liberdade dos usuários exige um esforço constante para evitar abusos. Ao utilizar o KrakenDNS, você concorda em respeitar os limites de uso ético e contribuir para um ecossistema DNS confiável.

**Pesquisadores responsáveis** são bem-vindos e podem entrar em contato diretamente conosco.

krakendnsserver@protonmail.com

## 🆕 Atualização de Infraestrutura – 10/06/2025

Olá, usuários do KrakenDNS! 🐙

Informamos que um novo servidor está **oficialmente online**:

- 🌍 **KrakenDNS Amsterdam**
- IP: 95.179.151.156

Nosso novo DNS foi otimizado para máxima estabilidade e responsividade, superando outras regiões e se tornando o **favorito da equipe KrakenDNS**.

## ⚠️ Atualização sobre o DNS de Chicago

Desativamos temporariamente o servidor de DNS em Chicago devido ao baixo tráfego e à migração para uma infraestrutura mais eficiente.

## ❌Relatório de Incidente – UpCloud (9 e 10 de Junho de 2025)

Após instabilidade, o servidor Frankfurt (Alemanha) foi restabelecido com sucesso pela DigitalOcean. Testamos a infraestrutura da UpCloud para expandir o KrakenDNS, mas infelizmente os servidores apresentaram inconsistência nas respostas DNS, falhas inesperadas e suporte limitado à personalização de rede. Decidimos encerrar as operações na UpCloud e focar em provedores confiáveis e escaláveis.

**Linha do tempo dos problemas:** 


**09/06** - Falha temporária no **sistema de pagamento** da UpCloud; saldo ficou marcado como “inválido”, causando instabilidade administrativa.                                             
**09/06 à noite** - VPS de **Finlândia** apresentaram sinais de lentidão. Testes com `dig` mostraram falhas de resposta e queries recusadas.                                             
**10/06 (manhã)** - As VPS foram **desligadas sem aviso claro**, embora o painel indicasse “ativo”. A comunicação com o suporte foi lenta e evasiva.                                                   
**10/06 (tarde)** - Suporte afirmou que não havia bloqueio de rede, mas os serviços continuavam **recusando requisições externas (status REFUSED)** mesmo com firewall correto.                        
**10/06 (noite)** - Confirmado que **não era um erro do AdGuardHome ou do sistema**. Diagnóstico aponta infraestrutura da UpCloud como instável e com comportamento inconsistente de rede. 

Devido a instabilidades graves nos VPS (não relacionadas à configuração local), suporte insuficiente e uma rede pública não confiável para DNS de produção, migrarmos para a DigitalOcean. Estamos buscando provedores na região entre Helsinki e Suécia para otimizar o serviço. Agradecemos a paciência e manteremos atualizações! 🦑 **Observação**: Apesar da fama de ser econômica e limitada, a **Contabo** enviou respostas consistentes e previsíveis, um destaque valioso.

## 🆕 Atualização de Infraestrutura – 12/06/2025
Olá, usuários do KrakenDNS!

Informamos que um novo servidor está oficialmente online:

🌍 KrakenDNS Suécia Linode
IP: 172.234.100.170

A migração para a Linode e DigitalOcean foi necessária devido às instabilidades na Upcloud e foi bem-sucedida. Ontem (11/06/2025) realizamos testes com nossas ferramentas, confirmando a estabilidade da Linode que tem se mostrado ótima com sua infraestrutura. Pedimos aos usuários que indiquem empresas confiáveis para fortalecer nosso DNS.

## 🆕 Atualização de Infraestrutura – 16/06/2025
Olá, usuários do KrakenDNS!

Informamos que um novo servidor está oficialmente online:

🌍 KrakenDNS Finlândia tietokettu
IP: 5.61.88.206
A Evolução Continua!

Temos o prazer de anunciar um marco importante na jornada do KrakenDNS. Nossa presença global está se expandindo com dois nós robustos na região nórdica! Essa mudança estratégica fortalece nossa infraestrutura, garantindo uma resolução de DNS mais rápida e confiável para usuários em toda a Europa.

**O objetivo é simples:** Garantir que os usuários tenham acesso a um DNS público, rápido e resiliente, independentemente de barreiras políticas ou geográficas. Isso é só o começo! Estamos explorando ativamente novas parcerias e regiões para expandir ainda mais a rede.

A **Tietokettu** é uma empresa fundada em 2019, focada em serviços de hospedagem como web hosting, VPS, servidores dedicados e até serviços de gaming. Eles operam a partir de um data center próprio em **Lempäälä**, **Finlândia**. As Avaliações em plataformas como Trustpilot (4.5 estrelas) e HostAdvice (4.9 estrelas) reforçam a reputação da Tietokettu como uma escolha confiável para o KrakenDNS.




