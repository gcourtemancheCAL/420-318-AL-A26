# Exercices

En considérant la topologie suivante : 

<img src="img/Pasted image 20260828123751.png" width="800" />

Pour chacune des questions suivantes, identifiez les paramètres pour les règles de pare-feu demandées.

**Assumez les règles par défaut suivantes sur l’ensemble des hôtes (SAUF le routeur)** :
- Toute connexion sortante est approuvée
- Toute connexion entrante est bloquée.
- N’assumez la présence d’aucune autre règle.

**Pour le routeur :**
- N'assumez aucune règle en entrée ni en sortie.
- Par défaut, il accepte de router tout le trafic.

**Nous allons prendre pour acquis la logique iptables :**
 - Les règles sont appliquées dans l'ordre qu'elles sont créés.
 - La première règle rencontrée qui s'applique est celle qui détermine l'action
 - Le pare-feu est *stateless*

**Consignes :**
- Indiquez la liste des règles nécessaires à l'application des contraintes. 
- Pour chaque règle, indiquez les paramètres nécessaires. 
- Gérez chaque question indépendamment des autres
- Écrivez les règles les plus précises répondant à la question
## Informations disponibles
- Adresses IP :
	- source et destination
- Direction : 
	- Entrée, INPUT, Rx : Pour les PDUs reçus par le système
	- Sortie, OUTPUT, Tx : Pour les PDUs envoyés par le système
	- FORWARD : Pour les paquet que le routeur doit router
- Protocole
	- On parle de l'attribut dans l'en-tête du paquet IP
	- Peut être l'un de TCP, UDP, ou ICMP
- Port 
	- source et destination
	- Seulement valide pour TCP et UDP
- Informations spécifiques au protocole de niveau 3 et 4
	- flags tcp, 
	- type de message icmp, 
	- ...
## Question 1

Nous voulons que l’hôte `Serveur` puisse recevoir des connexions ssh de n’importe quel hôte dans le même réseau que lui et uniquement dans le même réseau que lui.

## Question 2

Nous voulons que Alice puisse être cliente http de l’hôte `Serveur` uniquement de `Serveur`

## Question 3

Nous voulons que Bob puisse transférer des fichiers vers `Serveur` via ftp en mode actif. On veut que `Serveur` accepte les requêtes ftp de tout le monde.
## Question 4

On ne veut pas qu'Alice réponde aux pings qu'elle reçoit. On veut, cependant, qu'elle puisse ping `Serveur` et `DNS`

## Question 5

On ne veut pas autoriser les requêtes ICMP en provenance de l'extérieur du réseau mais on voudrait autoriser les réponses.
## Question 6

Concernant Bob : 
- On veut qu'il puisse envoyer des requêtes (et recevoir les réponses) https vers des systèmes à l'extérieur du lan
- Lui autoriser toute communication http et https à l'intérieur de LAN.
