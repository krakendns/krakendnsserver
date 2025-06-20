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



