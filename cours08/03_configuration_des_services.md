# Configuration des services

## Impact sur la sécurité

La configuration d'un service influence directement la surface d'attaque d'un serveur. [Plusieurs variantes du thème aparaissent sur le OWASP TOP 10 des vulnérabilités les plus communes.](https://top10.owasp.org/2021/) 

## Qu'est-ce qu'on regarde?

**Questions à se poser pendant un audit :**

- Est-ce que le service est nécessaire?
- Est-ce que les interfaces utilisées sont toutes nécessaires?
- Quelles méthodes d'authentifications sont utilisées?
- Avec quel compte le service s'exécute-t-il? 
- De quelles permissions disposent le service?
- Est-ce que le chiffrement est activé?
- Est-ce que le logging est adéquat?

## Quelques exemples

### apache2

**Points à valider :**

- Masquer les informations de version (`ServerTokens Prod`, `ServerSignature Off`);
- Désactiver les modules inutiles
- Utiliser HTTPS lorsque possible
- Utiliser des permissions minimales sur `/var/www`
- Assigner un utilisateur et un groupe non privilégié au processus
- Désactiver l'indexation de répertoires.

**Exemples :**

```apache
ServerTokens Prod
ServerSignature Off

<Directory /var/www> 
	Options -Indexes +FollowSymLinks 
	...
</Directory>
```

### openssh-server

**Points à valider :**

- Interdire la connexion directe de `root` (`PermitRootLogin no`);
- Privilégier l'authentification par clés publiques (`PubkeyAuthentication yes`) au lieu de l'authentification par mot de passe (`PasswordAuthentication no`)
- Limiter les utilisateurs autorisés (`AllowUsers` ou `AllowGroups`);
- Limiter les tentatives de connexions (`MaxAuthTries`).

**Exemples :**

```text
PermitRootLogin no
PasswordAuthentication no
AllowUsers alice bob
MaxAuthTries 3
```
