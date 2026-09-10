# Référence iptables

Attention : la documentation de iptables est divisée en plusieurs chapitres :
-        **man iptables**
-        **man** **iptables-extensions**

## Survol des commandes communes

``iptables-save`` : Imprime sur sa sortie standard une version restaurable de la configuration de iptables.

``Iptables-restore`` : Lit sur l’entrée standard une configuration de iptables. Compatible avec le format de sortie de iptables-save.

``iptables -L`` : Affiche les règles iptables

``iptables -P <CHAIN> <POLICY>`` : Change le « policy » de la chaine « chain ».

``iptables -N <CHAIN>`` : Crée une nouvelle chaine.

``iptables -X <CHAIN>`` : Supprime une chaine.

``iptables -F <CHAIN>`` : Supprime toutes les règles dans une chaine.

``iptables -D <CHAIN> <INDEX>`` : supprime la règle INDEX de la chaine CHAIN. Les indexes commencent à 1.

``iptables -A <CHAIN> …paramètres… -j TARGET`` : Ajoute une règle à la fin de la chaine CHAIN. Si la règle est rencontrée, saute à la cible TARGET. Les paramètres optionnels servent à identifier les paquets pour lesquels cette règle s’applique.

## Paramètres de règle

``-p <protocol>`` : Identifie le protocole(tcp, udp, …) pour lequel la règle s’applique. Un seul protocole peut être spécifié par règle.

 ``-s <address ou reseau>`` : Adresse source du paquet. Peut être une adresse fixe ou bien une adresse réseau si accompagné d’un masque.

``-d <address ou reseau>`` : Adresse destination du paquet. Peut être une adresse fixe ou bien une adresse réseau si accompagné d’un masque.

``--sport <port>. –dport <port>`` : Le port source et destination respectivement. Utilisable uniquement pour les protocoles tcp et udp (voir man iptables-extensions pour plus d’information)

``-i <interface>`` : Spécifie l’interface.

## Quelques exemples :

Ajoute une règle qui bloque tous les datagrammes UDP entrant à partir d’une adresse dans le réseau 192.168.0.0/24 :
``iptables -A INPUT -p udp -s 192.168.0.0/24 -j DROP``

Ajoute une règle qui bloque tous les segments TCP en sortie vers le port 421 :
``iptables -A OUTPUT -p tcp --dport 421 -j DROP``

Ajoute une règle qui accepte, en entrée, tous les segments TCP:
``iptables -A INPUT -p tcp -j ACCEPT``