# ÉTAPE 1 - Règles par défaut

## Policy
```
iptables -P INPUT DROP
iptables -P OUTPUT ACCEPT
```

## Trafic loopback

```bash
# -i lo = interface de loopback. trafic identifié par interface
# -s 127.0.0.0/8 =  network loopback. trafic identifié par ip source
# les critères s'additionnent (ET)

iptables -A INPUT -i lo -s 127.0.0.0/8 -j ACCEPT
```

# ÉTAPE 2 - HTTP

```bash
iptables -A INPUT -p tcp -s 192.168.0.0/24 --dport 80 -j ACCEPT
iptables -A INPUT -p tcp -s 192.168.0.X --sport 80 -j ACCEPT
```

# ÉTAPE 3 - SSH et SFTP

## Requetes SSH

```bash
iptables -A INPUT -p tcp -s 192.168.0.0/24 --dport 22 -j ACCEPT
```

## Reponses SSH

```bash
iptables -A INPUT -p tcp -s 192.168.0.0/24 --sport 22 -j ACCEPT
```

## Bloquer vers autres réseaux

### On commence par autoriser le réseau local

```bash
iptables -A OUTPUT -p tcp -d 192.168.0.0/24 --dport 22 -j ACCEPT
iptables -A OUTPUT -p tcp -d 192.168.0.X --dport 22 -j ACCEPT

### Ensuite on bloque le reste
iptables -A OUTPUT -p tcp --dport 22 -j DROP
```

# DNS

```bash
# Serveur DNS : TCP _et_ UDP port 53
iptables -A INPUT -p udp -s 1.1.1.1 --sport 53 -j ACCEPT
iptables -A INPUT -p tcp -s 1.1.1.1 --sport 53 -j ACCEPT

iptables -A INPUT -p udp -s 8.8.8.8 --sport 53 -j ACCEPT
iptables -A INPUT -p tcp -s 8.8.8.8 --sport 53 -j ACCEPT


## Meme principe que tantot : on va autoriser les requetes vers certains serveurs et bloquer le reste

# Autoriser les requêtes DNS vers les serveurs permis
iptables -A OUTPUT -p udp -d 1.1.1.1 --dport 53 -j ACCEPT
iptables -A OUTPUT -p tcp -d 1.1.1.1 --dport 53 -j ACCEPT

iptables -A OUTPUT -p udp -d 8.8.8.8 --dport 53 -j ACCEPT
iptables -A OUTPUT -p tcp -d 8.8.8.8 --dport 53 -j ACCEPT

iptables -A OUTPUT -p udp --dport 53 -j DROP
iptables -A OUTPUT -p tcp --dport 53 -j DROP
```