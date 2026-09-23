# Iptables - 1

 iptables est un logiciel permettant d’écrire des règles de pare-feu sur linux.

`iptables` utilise des `chains` de règles afin de déterminer comment les connexions ou les paquets vont être gérés. Les règles sont évaluées en ordre, et la première règle dans une chaine identifiant un paquet est appliquée.

3 chaines existent par défaut :

- `INPUT` : La chaine invoquée sur connexion entrante.
- ``OUTPUT`` : La chaine invoquée sur une connexion sortante.
- ``FORWARD`` : La chaine invoquée lorsqu’il est question de _packet forwarding_ (i.e. routing).

Des chaines additionnelles peuvent être créés et invoqués explicitement.

Une règle va être composée de paramètres identifiant un message ou une connexion, ainsi que d’une action. L’action va consister à ``jump`` vers une cible. Une cible peut être une action prédéfinie ou bien une autre chaine.

Les cibles suivantes existent et peuvent être utilisés comme action finale :
- ``ACCEPT`` : autorise le message
- ``DROP`` : rejette le message.

Les règles sont concaténées à la fin d’une chaine. L’ordre d’insertion des règles est donc important.

À tout moment, vous pouvez utiliser la commande ``iptables-save > firewall.rules`` pour sauvegarder votre configuration dans le fichier ``firewall.rules``.

Vous pouvez ensuite restaurer une configuration à l’aide de la commande ``iptables-restore < firewall.rules``.

Comme le fichier généré est un fichier texte, vous pouvez toujours simplement le modifier afin de changer l’ordre des règles dans une chaine.

**Il vous est recommandé de sauvegarder fréquemment votre configuration iptables.**

Vous pouvez toujours utiliser la commande ``iptables -L`` pour afficher vos règles.

## Préparation
Pour ce laboratoire, nous allons avoir besoin d'un minimum de un ordinateur portable avec une machine virtuelle linux. La machine virtuelle linux doit être connecté au réseau en mode pont / bridged. 

Utilisez une configuration DHCP.

Installez les services suivants sur votre système linux : 
- Un serveur http (apache2)
- Un serveur ssh (openssh-server)
-  iptables

## Important

- N'oubliez pas de tester vos configurations au fur et à mesure.
- Faites des copies régulières de vos configuration dans des fichiers différents.

## Étape 1 – Création des règles par défaut

Nous voulons, par défaut, refuser toutes les connexions entrantes. Nous allons donc utiliser le ``policy`` de la chaine appropriée afin de définir un comportement par défaut. Le ``policy`` d’une chaine est la cible utilisée si aucune règle n’a été trouvée - en d'autres mots, la règle par défaut.
-         Modifier le ``policy`` de la chaine ``INPUT`` pour ``DROP``
-         Modifier le ``policy`` de la chaine ``OUTPUT`` pour ``ACCEPT``

Nous allons vouloir autoriser toutes les connexions faites en loopback. **Créez une règle autorisant toutes les connexions entrantes sur le réseau 127.0.0.0/8.**

## Étape 2 – Configuration http

Ajoutez une règle ``iptables`` autorisant les requêtes http en provenance de n’importe quel hôte de votre réseau.

- Vous aurez besoin d’une règle entrante pour la réception des _requêtes_ http.
- Vous aurez besoin d’une règle permettant à votre serveur de recevoir les _réponses_ http aux requêtes qu’il se fait à lui-même. Faite attention :
	- On permet déjà la réception de tous les messages en provenance de l’hôte local. Pour quelle situation allons-nous devoir créer une nouvelle règle?
	- On ne veut pas permettre les _réponses_ http en provenance des autres hôtes de votre réseau. Comment allez-vous faire pour prévenir ce cas de figure?

## Étape 3 – Configuration ssh

Ajoutez les règles iptables permettant le fonctionnement de ssh et sftp dans les contextes suivants :

-         Nous voulons autoriser les connexions ssh en provenance de n’importe quel hôte de notre réseau.
-         Nous voulons pouvoir nous connecter à n’importe quel hôte dans notre réseau.
-         Nous voulons bloquer les connexions en provenance et en direction des autres réseaux.
-         Nous voulons pouvoir nous connecter par ssh à nous même sur n’importe quelle interface.

Les autres contextes doivent être bloqués par défaut.

## Étape 4 – Configuration dns

Ajoutez les règles iptables permettant le fonctionnement du dns dans le respect des contraintes suivantes :

- Nous voulons pouvoir faire des requêtes dns vers les serveurs suivants :
	- 8.8.8.8
	- 1.1.1.1
- Nous voulons bloquer les requêtes dns vers les autres serveurs
