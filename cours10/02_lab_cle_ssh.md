# Laboratoire - Authentification via clé SSH

## Objectif

L'objectif de ce laboratoire est de voir comment configurer l'authentification via clé ssh sur son serveur openssh-server.

Nous allons aussi améliorer la sécurité du serveur en autorisant uniquement les connexion faite via une clé ssh et en désactivant l'authentification par mot de passe.

### Pourquoi on ferait ça

- Une clé ssh est une preuve de possession.
- La clé ssh permet d'identifier le système utilisé en tant que tel
- C'est une preuve plus solide qu'une preuve de connaissance en réduisant les risques d'une attaque par force brute.
- Permet aussi d'automatiser des tâches (e.g. transfert de fichiers) sans avoir à partager de secrets dans ses scripts

## Prérequis

- Connecter son système linux au réseau de la classe.
	- La configuration IP se fait via DHCP.
	- Si vous utiliser une machine virtuelle, vous allez devoir mettre l'interface en mode d'accès *par pont* (bridged adapter)
- Installer openssh-server

## Comment fonctionne l'authentification par clé ssh

L'authentification par clé SSH utilise une paire de clés :
- une clé privée, gardée secrète sur le client
- une clé publique, copiée sur le serveur dans le fichier `~/.ssh/authorized_keys`

Quand l'utilisateur tente de se connecter, le client prouve qu'il possède la clé privée en effectuant un défi cryptographique avec la clé publique enregistrée sur le serveur. Le serveur vérifie la signature sans jamais avoir besoin de connaître la clé privée.

## Laboratoire

### Étape 1 - Générer une paire de clés SSH

Sur la machine cliente (la machine depuis laquelle on se connecte au serveur), générer une clé SSH :

```bash
ssh-keygen -t ed25519 -C "votre_nom@votre_machine"
```

Lors de la demande de mot de passe, vous pouvez laisser la commande vide si vous voulez une clé non protégée par mot de passe. 

>[!notification] La commande va demander/indiquer où les fichiers de clé vont être créés. Nous allons prendre pour acquis que le chemin par défaut est le `~/.ssh/`

Cette commande crée deux fichiers :
- `~/.ssh/id_ed25519` : la clé privée
- `~/.ssh/id_ed25519.pub` : la clé publique

### Étape 2 - Copier la clé publique sur le serveur

Le serveur doit connaître la clé publique pour autoriser la connexion. Transférez la clé publique générée sur le client vers le répertoire `~/.ssh/` dans le fichier `authorized_keys`. Utilisez le home de l'utilisateur vers lequel vous allez vouloir vous connecter.

>[!warning] L'authentification par clé ssh est faite sur une base utilisateur. C'est à dire que - sur un serveur donné - si l'utilisateur X1 dispose de la clé publique de C1 et que l'utilisateur X2 n'en dispose pas on peut s'attendre au résultat suivant : 
	-C1 peut se connecter avec sa clé ssh en tant qu'utilisateur X1 sur le serveur.
	-C1 ne pourra pas se connecter avec sa clé ssh en tant qu'utilisateur X2 sur le serveur. 

Vous pouvez faire le transfert en utilisant le client scp de votre choix. Par exemple, en ligne de commande : 

```bash
# Le : est _très_ important
scp ~/.ssh/id_ed25519.pub utilisateur@adresse_serveur:
# Il ne va plus que rester à déplacer la clé au bon endroit sur le serveur.
```

### Étape 3 - Tester la connexion sans mot de passe

Maintenant, essayer de se connecter au serveur :

```bash
ssh utilisateur@adresse_serveur
```

Si la clé a bien été copiée, vous devriez être connecté sans devoir entrer de mot de passe.

À partir de maintenant, la clé privée de votre client sert de preuve d'identité. Le serveur valide cette preuve à l'aide de la clé publique enregistrée.

### Étape 4 - Désactiver l'authentification par mot de passe

Les serveurs SSH sont souvent configurés pour accepter les deux types d'authentification : par mot de passe et par clé. Pour augmenter la sécurité, on peut forcer l'authentification uniquement par clé.

Modifier la configuration du serveur :

```bash
sudo nano /etc/ssh/sshd_config
```

Vérifier et modifier les lignes suivantes :

```bash
PubkeyAuthentication yes
PasswordAuthentication no
```

Si certaines lignes sont absentes, les ajouter. Ensuite, sauvegarder le fichier puis redémarrer le service SSH