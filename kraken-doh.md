## Kraken DoH Server

**Pré-requisitos**
- Servidor Linux (Ubuntu/Debian/ e outros)
- Domínio configurado
- Portas 80/443 liberadas

**Primeiros Passos**

**1. Liberar Porta HTTP**
```bash
# Firewall UFW
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

# Firewall iptables (alternativo)  
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT
```

## Instalar Certbot

```bash

# Ubuntu/Debian
sudo apt update
sudo apt install certbot

# CentOS/RHEL
sudo yum install certbot
```

## Gerar Certificado SSL

```bash
sudo certbot certonly --standalone -d seudominio.com
```

## Se tudo funcionar

/etc/letsencrypt/live/seudomínio/fullchain.pem

/etc/letsencrypt/live/seudomínio/privkey.pem

## Status Atual

Atualmente, o serviço está funcionando perfeitamente em:
- **Mikrotik RouterOS** 
- **Browsers** (Firefox, Chrome)
- **Desktops** com configurações manuais de DNS

⚠️ **Nota**: O suporte ao Android ainda está em desenvolvimento.


## Instalação Básica: Caddy + dnsdist (para DNS público com DoH)

⚠️ **Atenção**: Esse é apenas um guia de instalação genérico, não contém detalhes de produção ou configurações específicas de segurança utilizadas pelo KrakenDNS. Para produção consulte documentação oficial.

**Instalando o Caddy Debian / Ubuntu**

```bash
sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | sudo tee /etc/apt/trusted.gpg.d/caddy.asc
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy.list
sudo apt update
sudo apt install caddy
```

**Instalando o Caddy no RHEL**

```bash
dnf install 'dnf-command(copr)'
dnf copr enable @caddy/caddy
dnf install caddy
```

## Instalando o dnsdist

**Instalando o dnsdist Debian / Ubuntu**

```bash
sudo apt install dnsdist
```

**Instalando o dnsdist RHEL**

```bash
sudo dnf install epel-release
sudo dnf install dnsdist
```

 ## Exemplo de configuração básica de Caddy + dnsdist (com DoH)

 **⚠️ Importante:** Essas configurações são exemplos educativos, com portas e caminhos fictícios.

 **Exemplo de configuração Caddy (para DoH)**

```bash
 Caminho: /etc/caddy/Caddyfile

https://seudominio.com:1010 {
    tls /etc/letsencrypt/live/seudominio.com/fullchain.pem /etc/letsencrypt/live/seudominio.com/privkey.pem

    route /dns-query* {
        reverse_proxy 127.0.0.1:1053
    }

    encode gzip
    log {
        output file /var/log/caddy/doh-access.log
    }
}

```

**Exemplo de configuração dnsdist (modo DoH)**


```bash
-- Habilitar DoH (DNS over HTTPS)
addDOHLocal("0.0.0.0:1053", "/etc/letsencrypt/live/seudominio.com/fullchain.pem", "/etc/letsencrypt/live/seudominio.com/privkey.pem")

-- Upstream DNS servers (exemplo apenas)
newServer("9.9.9.9")
newServer("1.1.1.1")
newServer("8.8.8.8")

-- Policies básicas
setServerPolicy(firstAvailable)

-- Logging básico
setVerboseHealthChecks(true)

```







