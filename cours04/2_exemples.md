# Exemples 

Afin de simplifier le modèle, les exemples ci-dessous vont utiliser les règles de priorité d'un pare feu `iptables` (sur linux). C'est à dire que les règles sont appliqués en ordre.

La syntaxe utilisé ici est très approximative - l'objectif est d'illustrer les filtres utilisés et non pas une syntaxe particulière.

## Exemple 1 : On veut bloquer tout trafic TCP vers internet sauf pour les connexions SSH sortantes

```
OUTPUT TCP dst_addr 192.168.0.0 255.255.0.0 ACCEPT # On accepte le trafic TCP local
INPUT TCP src_addr 192.168.0.0 255.255.0.0 ACCEPT
OUTPUT TCP dst_port 22 ACCEPT
INPUT TCP src_port 22 ACCEPT
DEFAULT DROP
```

## Exemple 2 : On veut permettre seulement la navigation web via HTTPS

```
OUTPUT TCP dst_port 80 DROP
INPUT TCP src_port 80 DROP
OUTPUT TCP dst_port 443 ACCEPT
INPUT TCP src_port 443 ACCEPT
```

## Exemple 3 : On veut autoriser uniquement les requêtes DNS vers un serveur précis

```
OUTPUT UDP dst_addr 8.8.8.8 255.255.255.255 dst_port 53 ACCEPT
INPUT UDP src_addr 8.8.8.8 255.255.255.255 src_port 53 ACCEPT
```

## Exemple 4 : On veut exposer un serveur web au réseau local seulement

```
INPUT ANY src_addr 192.168.0.0 255.255.0.0 ACCEPT
DEFAULT DROP
```
