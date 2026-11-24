# Laboratoire : Chiffrement et hachage

L'objectif du laboratoire consiste à automatiser un système de gestion des backups primitifs. Ce laboratoire va vous amener à pratiquer des opérations de chiffrement et de hachage afin de sécuriser la confidentialité des données et d'en valider l'intégrité.

## Politiques de backup

Un bon backup est une copie des données qui permet de les récupérer après une suppression accidentelle, une panne, un vol ou une attaque par ransomware.

### Les trois caractéristiques d’un bon backup

- **Complet** : un bon backup contient toutes les données nécessaires
- **À jour** : un bon backup est faire assez souvent pour limiter les pertes potentielles
- **Restaurable** : Un bon backup permet réellement la récupération des données

### La règle 3-2-1

- **3 copies** des données au total, soit l’original et deux backups.
- **2 types de supports** différents pour répartir le risque.
- **1 copie hors site** pour limiter les conséquences d'un incident

**Exemple** — Garder les fichiers de travail sur le SSD de l’ordinateur, un backup sur un disque dur externe et un autre dans le cloud.

## Topologie du laboratoire

<img src="img/Pasted image 20261104140121.png" width="600" />

Nous allons configurer une tâche automatisée de backup sur le serveur S1. Les backups vont être entreposés sur le serveur S2.

## Résumé des tâches à accomplir

**Vous allez devoir :**
 - Vous connecter par SSH au serveur S1 afin de le configurer.
 - Faire les opérations nécessaires afin de permettre à S1 de se connecter via SSH à S2.
	 - Nous allons permettre l'authentification par clé ssh afin de permettre l'automatisation de transfert de fichiers via scp.
 - Utiliser cron pour céduler automatiquement des opérations de backups.
 - Chiffrer des archives en utilisant gpg.

## Étapes du laboratoire

### Étape 1 - Connexion à S1

Connectez vous au réseau tel qu'indiqué en classe.

Connectez vous par SSH au serveur S1 :
- Nom d’utilisateur : m\<matricule\>. E.g. **m123456**
- Mot de passe : nom de famille, en minuscule, sans espace

### Étape 2 - Rédaction du script d'archivage

Rédigez un script qui va effectuer les opérations suivantes : 
- Créez une archive compressée au format `.tar.gz` du dossier `/data`
- Le nom de l'archive devrait contenir la date du jour et votre nom d'utilisateur. E.g. : `backup-m1234567-2026-11-04.tar.gz`

Testez votre script.

### Étape 3 - Chiffrement de l'archive

Afin de préserver la confidentialité de nos copies de sauvegarde, nous allons vouloir chiffrer l’archive précédemment créé.

Nous allons vouloir éviter de mettre le mot de passe directement en ligne de commande. Dans cet objectif, nous allons utiliser un fichier contenant le mot de passe utilisé pour le chiffrement.
- Créé le fichier **~/password-backup** avec le texte suivant : « **super-mot-de-passe -2000** ».

Modifiez ensuite votre script d'archivage afin d'y ajouter une commande similaire à celle ci : 
`gpg --batch --passphrase-file ~/password-backup --symmetric --cipher-algo AES256 -o LE-FICHIER-EN-SORTIE LE-FICHIER-EN-ENTRÉE`

Remplacez les noms de fichier afin de chiffrer votre archive nouvellement créé. Utilisez l'extension `.gpg` pour le fichier nouvellement créé.

L’argument **--batch** fait en sorte que **gpg** ne va pas demander d’interaction manuelle. C’est nécessaire lorsque **gpg** est utilisé dans un script.

L’argument **--passphrase-file** indique d’utiliser le mot de passe contenu dans un fichier.

gpg va chiffrer les données du fichier `LE-FICHIER-EN-ENTRÉE` et écrire le résultat dans un nouveau fichier au nom de `LE-FICHIER-EN-SORTIE`

**Question :** Pourquoi est-ce qu'on utilise un algorithme de chiffrement symétrique? Qu'est-ce que ça veut dire? 

### Étape 4  - Clé SSH

L’authentification au serveur se fait présentement par le biais de mot de passe. Cette approche ne va pas fonctionner dans un contexte non interactif. Pour cette raison, nous allons vouloir faire en sorte que S1 puisse se connecter à S2 en utilisant une clés SSH.
- Sur S1 : Générez les clés ssh.
- Partagez ces clés avec S2.

**NB : Connectez-vous à S2 en utilisant le nom d'utilisateur `exbackup` et le mot de passe `backuppw`.**

>[!important] Vous devez ajouter (append) votre clé publique au fichier `~/.ssh/authorized_keys`. Si vous remplacé (overwrite) ce fichier, vous allez briser le travail des autres.

### Étape 5  - Transfert de l'archive

Modifiez votre script d'archivage afin de : 
- Transférer l'archive chiffrée sur S2.
- Supprimer les fichiers intermédiaires qui ont été créés sur S1
	- Le .tar.gz
	- Le .gpg

Tester votre script et valider le résultat. 
- Vous allez devoir télécharger l'archive chiffrée qui a été transférée
- Vous allez devoir la déchiffrer afin d'en valider le contenu

Déchiffrez l'archive avec la commande suivante :  
`gpg --batch --passphrase-file ~/password-backup --decrypt --cipher-algo AES256 -o LE-FICHIER-EN-SORTIE LE-FICHIER-EN-ENTRÉE`

Faites l'extraction des fichiers de l'archive.

Utilisez md5sum afin de générer une signature des fichiers résultants et comparez cette signature à celle des fichiers d'origines.

### Étape 6  - Céduler le backup

Ajouter une tâche à votre crontab afin d'exécuter votre tâche de backup à tous les samedi au matin.