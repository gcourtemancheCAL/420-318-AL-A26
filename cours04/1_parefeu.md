# Pare feu

**Définition :** Un pare feu est une couche de protection qui opère en filtrant le trafic réseau. Des règles sont utilisées afin de déterminer si une trame ou un paquet va être autorisé ou bien refusé.

Une règle se définit par une liste de critère identifiant si la règle s'applique au trafic suivit d'une action à entreprendre.

## Direction

Rx (Entrant) : La règle identifie le trafic entrant sur le système.

Tx (Sortant) : La règle identifie le trafic sortant du système

Fwd (forwarding) : La règle identifie le trafic qui passe par le système sans être en destination du système (e.g router)

## Layer 2

La couche 2 de la pile OSI est la couche liaison. Un pare feu de niveau 2 va filtrer les trames Ethernet. Les paramètres utilisés sont ceux de niveau 2 : soit les adresses MAC.

### Paramètres de niveau 2
- Adresse MAC d'origine
- Adresse MAC de destination

## Layer 3/4

Les pare feu de niveau 3/4 vont opérer sur les paquets, les segments et les datagrammes. Comme la paquet IP doit être inspecté pour identifier les éléments de niveau 4, typiquement les fonctions de niveau 3 et 4 sont combinées.

### Paramètres de niveau 3
- Adresse IP d'origine
- Adresse IP de destination
- Protocole (TCP, UDP, ...)

Des masques peuvent être utilisés pour couvrir une plage d'adresse

### Paramètres de niveau 4
- Port d'origine
- Port de destination
- Les flags TCP
- La taille
- ...

Un pare feu 'stupide' pourrait autoriser uniquement les connexions tcp sortantes en bloquant les SYN entrant.

## Les actions
Tous les pare feu ne supportent pas nécessairement les mêmes actions. De façon générale, les actions suivantes peuvent être vues :
- DROP : "Drop" le paquet silencieusement.
- REJECT : Refuse le paquet et envoie notification au pair si possible 
- ACCEPT : Accepte le paquet et continue à le passer à travers la stack TCP/IP

## Stateful et stateless

### Stateless

Un pare feu dit `stateless` est un pare feu qui ne suit pas le statut des connexions. Un pare feu `stateless` évalue chaque paquet indépendamment des autres reçus.

### Stateful

Un pare feu dit `stateful` va suivre l'état d'une communication et évaluer les paquets en tenant compte d'un contexte établie. Un pare feu `stateful` est assez intelligent pour déterminer, par exemple, qu'un segment reçu est associé à une connexion TCP qui a été précédemment approuvé.

Un pare feu `stateful` est plus intelligent mais demande plus de ressources. Il faut néanmoins noter que dans certains cas même les pare feu les plus intelligents peuvent rencontrer des situations ambigus.  


## Pare feu microsoft defender

https://learn.microsoft.com/en-us/windows/security/operating-system-security/network-security/windows-firewall/rules

Pare feu stateful : si une transaction sortante est autorisée, tous les paquets associés sont autorisés.

Particularité : Peut donner des permissions à des programmes spécifiques.

**Priorité :** 
1. Explicite bloque
2. Explicite accepte
3. Règle par défaut

