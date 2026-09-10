
TODO Ajouter logging.

# Iptables - 2

## chain

Les chains ``iptables`` représentent un ensemble de règle. Nous avons utilisés certaines chaîne par défaut qui sont invoqués automatiquement à des moments précis : `INPUT` et `OUTPUT`.

Nous pouvons créer des chaines "custom" pour regrouper des règles. Ces chaines peuvent être invoqués explicitement via d'autres chaines.

Il y a plusieurs avantages à travailler ainsi : 
- S'organiser, tout simplement.
- Permet une écriture plus modulaire des règles.
	- On peut après plus facilement ajuster nos politiques
- Facilite la gestion des règles dynamiques.

## cron

``cron`` est un outil permettant de céduler des tâches qui vont se répéter à des intervalles précis sur un système. ``cron`` va rouler toutes les commandes de votre ``crontab`` sous votre utilisateur.

Le service ``cron`` va lire un fichier ``crontab`` afin de trouver les règles définissant les différentes tâches à exécuter. Vous pouvez modifier ce fichier avec la commande ``EDITOR=/usr/bin/nano crontab -e``. Chaque utilisateur est propriétaire de son propre ``crontab``. Les commandes sont exécutés avec l'utilisateur associé. 

Il est important de préciser l’éditeur à utiliser sinon vous risqué de vous perdre dans la prison de Vim.

<img src="img/Pasted image 20260831140330.png" width="800" />

**Question** : Comment fait l'expression ``EDITOR=/usr/bin/nano`` pour préciser l'éditeur qu'on utilise?

Dans un fichier ``crontab``, chacune des lignes représente une tâche à accomplir.

Le format d’une ligne est le suivant :

``minute heure jour-du-mois mois jour-de-la-semaine commande et ses arguments``

Pour le moment d’exécution : on peut inscrire une valeur précise ou un astérisque pour n’importe quelle valeur. ``cron`` va comparer l’heure et la date actuelle à ces valeurs, et exécuter la commande donnée lorsqu’il y a concordance.

Ainsi la ligne :

``0 * * * * touch /home/user/a``

Va exécuter la commande « ``touch /home/user/a`` » à toutes les heures (la minute 0, de chaque heure, de chaque jour).

**Attention** – la ligne suivante va s’exécuter à toutes les minutes de l’heure 1 de tous les jours :

``* 1 * * * touch /home/user/a``

Si on veut en restreindre l’exécution à une fois par heure, il va falloir préciser la minute à laquelle on veut l’exécution. Il en va de même pour les jours du mois et les jours de la semaine. Cette règle s’exécuterait donc à 1h30 tous les jours.

``30 1 * * * touch /home/user/a``



## Étape 1 - Réécriture des règles sous forme de chaines

Réécrivez les règles du laboratoire précédent en encapsulant chaque étape dans sa propre chaine.

## Étape 2 – Introduction cron

Pour se pratiquer, nous allons commencer en rédigeant un script trivial qui s'exécute régulièrement. 

Le script va devoir ajouter, à la fin du fichier ``cron.log`` se trouvant dans le home de l'utilisateur qui lance le script, une ligne contenant le nom d'utilisateur, la date et l'heure (hh:mm:ss).

Appelez le script ``timestamp.sh`` et placez le dans votre home. Ajoutez une entrée dans votre ``crontab`` exécutant ce script aux 5 secondes.

Une fois le bon fonctionnement de la chose validée, modifier le script afin de qu'il s'exécute aux 30 minutes.

## Étape 2 – Ban automatique

Nous allons vouloir créer une tâche qui bannit temporairement les individus ayant échouer à se connecter via ``ssh`` trop souvent dans un court laps de temps.

Nous allons commencer par faire quelques modifications à notre configuration iptables :
- Créez une nouvelle chaine iptables « tmpban »
- Dans la chaine « INPUT », ajoutez un jump vers la chaine « tmpban » de sorte à ce que « tmpban » soit évalué avant toutes les autres règles.

**_NB_** _: N’hésitez pas à sauvegarder votre configuration iptables dans un fichier et d’en modifier l’ordre des règles manuellement au besoin._

Ensuite, nous allons créer un script « ``chkban.sh`` » qui identifie les hôtes problématiques

- Un hôte est jugé problématique s’il a échoué à se connecter au serveur ssh 3 fois ou plus dans l’heure courante.
- Chaque hôte problématique va être ajouter à la fin d’un fichier texte « current » dans votre home.
- Le script doit se situer dans votre home.

Ensuite, nous allons créer un script « ``bannew.sh ``» bloque directement l’adresse IP des hôtes problématiques par le biais de règles dans la chaine « ``tmpban`` :

- Commencez par renommer le fichier « ``current`` » à « ``last`` ».
- Ensuite, appelez le script « ``chkban.sh`` ».
- Obtenez les nouvelles adresses à bannir en comparant « ``current`` » et « ``last`` ».
	- Utilisez la commande ``comm`` pour faire la comparaison
- Bannissez ces nouvelles adresses en ajoutant les règles nécessaires à la chaine « ``tmpban`` ». Évitez de bloquer la même adresse ``ip`` en double.
- Considérez le cas où ni « ``current`` » ni « ``last`` » n’existent au lancement du script. Le script devrait quand même fonctionner.

N'oubliez pas de testez votre script.

Écrivez un script « ``resethr.sh`` » :
- Ce script devra supprimer toutes les règles de la chaine « tmpban »
- Assurez-vous aussi de supprimer les fichiers « current » et « last ».

Finalement, ajoutez les entrées cron suivantes :
- Les entrées permettant d’identifier et bannir les utilisateurs suspects aux minutes 15, 30, et 45 de chaque heure.
- Une entrée permettant de remettre à 0 la liste d’adresse bannie à la minute 0 de chaque heure.