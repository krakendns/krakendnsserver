## Como funciona o DNSCrypt dentro do KrakenDNS? Antes de tudo:

Esta documentação tem **fins educativos**. Não representa a **configuração real dos nossos servidores**.
Para configurações detalhadas e seguras, consulte sempre a documentação oficial do projeto DNSCrypt-Proxy:

```bash

https://github.com/DNSCrypt/dnscrypt-proxy

https://www.dnscrypt.org/

```

**Fluxo de criptografia**: Usuários → KrakenDNS Frankfurt (199.247.21.0) → DNSCrypt-proxy ou Cloudflare


```bash
sudo nano /etc/dnscrypt-proxy/dnscrypt-proxy.toml

```

```bash
server_names = ['cloudflare']

listen_addresses = ['127.0.0.1:53']

bootstrap_resolvers = ['9.9.9.9:53', '149.112.112.112:53', '1.1.1.1:53', '1.0.0.1:53']

ignore_system_dns = true

block_ipv6 = true

skip_incompatible = false
require_dnssec = false
require_nolog = false
require_nofilter = true

dnscrypt_ephemeral_keys = true # Renovação automática de chaves para Forward Secrecy
tls_cipher_suite = [52393, 52392] # TLS_AES_256_GCM_SHA384, TLS_CHACHA20_POLY1305_SHA256

force_tcp = true
tls_disable_session_tickets = true

lb_strategy = 'p2'
lb_estimator = true

log_level = 2

[static]

[static.'controld']
  stamp = 'Configuração personalizada - ControlD'

```
**O DNSCrypt** é uma camada adicional de segurança que protege as comunicações DNS entre nossos servidores e os resolvedores upstream, garantindo a criptografia, ofuscação, melhor controle sobre upstreams e Balanceamento de carga.

**Atenção sobre provedores privados como o ControlD**: Caso queira criar sua própria configuração com serviços como o ControlD, é necessário ter uma conta com um valor mensal a partir de **$2.00** 

```bash
https://controld.com/plans?step=plans&type=select
```

![image](https://github.com/user-attachments/assets/d38f8d46-e50e-4e97-98b6-55e86ab18bb7)

![image](https://github.com/user-attachments/assets/cdca64a8-f350-409f-85db-a9682f81a61f)


## Cloudflare Proxy Fallback DoH

O cloudflared atua como um **proxy local DoH**, convertendo consultas DNS tradicionais (porta 53) em requisições HTTPS criptografadas para os servidores Cloudflare.

 **Exemplo de configuração:**
 
```bash
 sudo nano /etc/cloudflared/doh-fallback.yml
```

```bash

# Configuração de proxy DNS local com fallback para DoH na Cloudflare

proxy-dns: true  # Habilita modo proxy DNS
proxy-dns-port: 53 # Porta local para receber consultas
proxy-dns-address: 127.0.0.1 # Configuração apenas em localhost (segurança)

# Servidores upstream Cloudflare via DoH
upstream:
  - https://1.1.1.1/dns-query # Cloudflare Primary (Anycast)
  - https://1.0.0.1/dns-query # Cloudflare Secondary (Redundância)

```

**Fluxo de Funcionamento:**

```bash
Cliente → cloudflared (127.0.0.1:53) → DoH HTTPS → Cloudflare (1.1.1.1)
```



| Item                   | Função                                                                                                      |
| ---------------------- | ----------------------------------------------------------------------------------------------------------- |
| **proxy-dns: true**    | Ativa o modo de Proxy DNS local                                                                             |
| **proxy-dns-port: 53** | Faz o proxy escutar na porta DNS padrão (53) apenas localmente (127.0.0.1)                                  |
| **upstream:**          | Define os servidores upstream (Cloudflare DoH) para onde as consultas serão enviadas com criptografia HTTPS |

**Benefícios da Configuração:**

- **Criptografia:** Todas as consultas vão via HTTPS
- **Performance:** Cache local + Anycast Cloudflare  
- **Redundância:** Dois servidores upstream
- **Compatibilidade:** Apps legados podem usar porta 53 local

**Comando de inicialização**

Executar cloudflared como serviço

```bash
sudo cloudflared service install --config /etc/cloudflared/doh-fallback.yml
sudo systemctl start cloudflared
```

## Comparação Técnica: DNSCrypt vs Cloudflared

Este documento compara as duas principais abordagens de criptografia DNS utilizadas no KrakenDNS para fins educativos.

**⚠️ Aviso: Este documento tem fins educativos. Não representa a configuração real dos nossos servidores.**

**DNSCrypt-Proxy**

**Arquitetura:** Usuário → DNSCrypt-Proxy → Múltiplos Resolvers → Internet

**Protocolo de Criptografia** 

**Algoritmo**: Curve25519 (ECDH) + XSalsa20Poly1305
**Autenticação**: Ed25519 signatures
**Forward Secrecy**: Chaves efêmeras renovadas automaticamente
**Protocolo**: DNSCrypt v2 (RFC draft)

**Vantagens DNSCrypt**

| Aspecto          | Benefício                             |                                                                                       
| -------------    | -------------------                   |
| Diversidade      | Suporta centenas de resolvers públicos|
| Anonimato        | Suporte a relay chains (Tor-like)     |
| Performance      | Load balancing automático entre resolvers|
| Flexibilidade    | Filtros por país, logs, DNSSEC, etc.|
| Auditabilidade   | Protocolo aberto, implementação open-source|


**Desvantagens DNSCrypt**


| Aspecto          | Limitação                  |                                                                                       
| -------------    | -------------------        |
| Complexidade     | Configuração mais elaborada|
| Compatibilidade  | Nem todos resolvers suportam todas features|
| Overhead         | Múltiplas camadas de criptografia|


## Cloudflared (DoH Proxy)

**Arquitetura:** Usuário → Cloudflared → Cloudflare (DoH/HTTPS) → Internet

**Protocolo de Criptografia**

**Algoritmo**: TLS 1.3 (ECDHE + AES-GCM/ChaCha20-Poly1305)
**Autenticação**: Certificados X.509 (CA hierarchy)
**Forward Secrecy**: Via ECDHE key exchange
**Protocolo: DNS-over-HTTPS** (RFC 8484)

**Vantagens Cloudflared (DoH Proxy)**


| Aspecto          | Benefício                             |                                                                                       
| -------------    | -------------------                   |
| Simplicidade     | "plug-and-play"                       |
| Performance      | Rede Anycast global otimizada         |
| Confiabilidade   | 99.99%+ uptime, infraestrutura enterprise|
| Compatibilidade  | Funciona com qualquer cliente DNS.       |
| Manutenção       | Zero configuração após setup inicial     |


**Desvantagens Cloudflared (DoH Proxy)**


| Aspecto          | Limitação                             |                                                                                       
| -------------    | -------------------                   |
| Dependência      | Dependência exclusiva da Cloudflare   |
| Privacidade      | Logs centralizados (mesmo que temporários) |
| Flexibilidade    | Sem opções de filtros personalizados       |


**Comparação Detalhada**


| Critério                | DNSrypt             | Cloudflared          |
| ----------------------- | -------------------- | -------------------- |
| Algoritmo Principal     | XSalsa20Poly1305    | AES-256-GCM / ChaCha20 |
| Key Exchange            | Curve25519          | ECDHE P-256/X25519   |
| Forward Secrecy         | Chaves efêmeras     | ECDHE                |
| Autenticação            | Ed25519 signatures  | X.509 certificates   |
| Resistência Quântica    | Parcial (Curve25519) | Parcial (ECDHE)      |
|                         |                     |                      |


**Performance e Latência**

Benchmark Médio (ms):
┌─────────────────┬──────────┬───────────┐
│ Método          │ Latência │ Overhead  │
├─────────────────┼──────────┼───────────┤
│ DNS Tradicional │    15    │     0%    │
│ DNSCrypt        │    25    │   +67%    │
│ Cloudflared DoH │    22    │   +47%    │
└─────────────────┴──────────┴───────────┘

## Casos de Uso Recomendados


**Use DNSCrypt quando**:

1- Precisar de máxima privacidade e anonimato
2- Diversidade de resolvers
3- Filtros específicos (malware, ads, etc.)
4- Ambientes restritivos
5- Prioridade e auditabilidade do protocolo

**Use Cloudflared quando:**

1- Simplicidade operacional
2- Máxima performance 
3- Prefere manutenção mínima

## Arquitetura Híbrida (Recomendada)

Produção: Cloudflared (performance + confiabilidade)
    ↓
Fallback: DNSCrypt (diversidade + privacidade)

## Conclusão

Não existe "melhor" opção. A escolha depende dos seus requisitos específicos:

1- Performance/Simplicidade → Cloudflared
2- Privacidade/Flexibilidade → DNSCrypt
3- Produção Crítica → Arquitetura híbrida

## Recomendação KrakenDNS

Para máxima robustez, implemente ambos com fallback automático, proporcionando o melhor dos dois mundos.









