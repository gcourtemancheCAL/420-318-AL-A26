# Labo: Identifier les serveurs SSH sur le reseau local

## Objectif

L'objectif de ce laboratoire est d'utiliser les outils vues en classe afin de faire l'audit d'un système sur le réseau.

## Prérequis
- Connecter son système linux par câble au réseau de la classe.
	- La configuration IP se fait via DHCP.
	- Si vous utiliser une machine virtuelle, vous allez devoir mettre l'interface en mode d'accès *par pont* (bridged adapter)
- Installer nmap
	- `apt install nmap`

## Étape 1 - Decouvrir le système cible

Il existe sur le réseau un système linux que nous allons vouloir auditer. Nous ne connaissons pas son adresse IP.

Nous allons vouloir faire un _scan_ du réseau afin d'essayer de localiser le serveur.

Les paramètres qui nous intéresse : 
- il roule un serveur ssh sur le port 2222
- on peut se connecter à ce serveur ssh avec l'utilisateur `exaudit` et le mot de passe `exodia`

Nous allons commencer par faire un scan rapide des système sur le réseau local : 

```bash
# Essaie de découvrir les systèmes actifs sur le réseau en utilisant des paquet 
# icmp
nmap -sn 192.168.0.0/24
```

Cette information est intéressante mais de peu d'utilité dans notre cas. Nous allons utiliser un scan différent pour identifier les hôtes dont le port 2222 est ouvert. 

```bash
# Port 2222 ouvert dans le réseau 192.168.0.0/24
nmap -p 2222 --open 192.168.0.0/24
```

Une fois les systèmes disposant d'un serveur ssh identifiés, nous allons nous y connecter avec les paramètres données. 

>[!warning] Il est possible que vous ayez plusieurs résultats : certains de vos collègues peuvent - pour une raison ou une autre - avoir le port 2222 d'ouvert. Le cas échéant, vous allez devoir essayer les différentes adresses ip identifiées afin de découvrir laquelle est la bonne. 

## Etape 2 - Audit des services

Pour cette section, référez vous au contenu du cours08.

### 2.1 Lister les services avec systemctl

En utilisant systemctl, faites la liste des services les plus intéressant à auditer. Normalement, les services qui utilisent le réseau sont particulièrement intéressant.

### 2.2 Vérification des ports ouverts

En utilisant  `ss`, identifiez les ports en écoute et ainsi que les processus associés. 

Comparez cette liste à votre liste de service obtenue précédemment. 
- Y a-t-il des processus qui ne figurent pas à votre liste de service?
- Est-ce que tout vos services sont présents dans cette liste? 
- Écoutent-ils tous sur le port standard?

### 2.3 Vérification des journaux 

Utilisez journalctl afin de valider les évènements récents. 

Identifiez les évènements notables. Normalement les tentatives d'accès refusées répétées sont particulièrement intéressantes. 

### 2.4 Vérification de la configuration des services

Pour chaque service critique identifié, vérifier sa configuration et identifiez les éléments pouvant poser des risques de sécurité.