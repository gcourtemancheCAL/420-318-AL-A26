
# Exercices - Sudo et sudoers

## Important

- Les modifications à `/etc/sudoers` doivent **toujours** être faites avec `visudo`, jamais directement avec un éditeur.
- Sauvegardez l'état initial de `/etc/sudoers` avant de commencer.
- Il est possible que sur certaines distributions vous ayez à installer `sudo`.

---

## Mise en place de l'environnement

Créez les trois utilisateurs normaux (sans accès root) suivants :
	1. alice
	2. bob
	3. carl

Ces 3 utilisateurs devraient pouvoir se connecter normalement avec mot de passe et avoir accès à leur home. 

---

## Création du script de démonstration

Objectif : créer un script test qui sera géré par différents utilisateurs.

1. Créez le fichier `/usr/local/bin/ex_sudo.sh` avec l'utilisateur `alice` :

```bash
#!/bin/bash
echo "=== Wattatow! Il s'en passe des affaires! ==="
id
echo "Répertoire courant : $(pwd)"
echo "Date : $(date)"
echo "HOME : $HOME"
echo "=== Ben c'est ça qui est ça ==="
```

2. Rendez le script exécutable par son propriétaire seulement :

3. Exécutez le script avec alice.

---

## Exercice 3 - Sudo sans mot de passe pour une tâche limitée

Objectif : permettre à `bob` de lancer le script en tant que `root` sans mot de passe.

1. Éditez la configuration sudoers. Rappel : ni alice, ni bob, ni carl ne peuvent utiliser sudo de façon générale. 

```bash
sudo visudo
```

2. Ajoutez la ligne suivante à la fin du fichier  :

```text
bob ALL=(root) NOPASSWD: /usr/local/bin/ex_sudo.sh
```

3. Comment lire cette règle :
   - `bob` : l'utilisateur auquel s'applique la règle
   - `ALL` : sur tous les hôtes
   - `(root)` : peut exécuter en tant que l'utilisateur root
   - `NOPASSWD` : sans demander le mot de passe
   - `/usr/local/bin/ex_sudo.sh` : uniquement ce fichier

3. Connectez vous en tant que `bob` et lancez votre script. 
	1. Essayez d'abord sans `sudo`
	2. ... et ensuite avec.

4. Quel utilisateur et uid sont affichés par le script?

5. Essayez de lancer d'autres commandes avec sudo. Est-ce que vous pouvez?
6. Essayer de votre script sous l'identité d'alice. En êtes vous capable?
	1. `sudo -u alice /usr/local/bin/ex_sudo.sh`

---

## Exercice 4 - Sudo avec mot de passe pour délégation d'un autre utilisateur

Objectif : permettre à `carl` de lancer le script en tant que `alice`, avec mot de passe requis.

1. Éditez `/etc/sudoers` à nouveau et ajoutez la ligne :

```text
carl ALL=(alice) /usr/local/bin/backup.sh
```

3. Comment lire cette règle :
   - `carl` : l'utilisateur auquel s'applique la règle
   - `(alice)` : peut exécuter en tant que l'utilisateur alice
   - Le mot de passe de carl sera **requis** (pas de `NOPASSWD`)
3. Testez en tant que `carl` 

## Exercice 5 - Limites de sudo

1. Créez un script appartenant à `alice:alice` au nom de `ex5.sh` dans le répertoire home d'alice. Le script devrait simplement contenir la commande `echo exercice 5`
2. Donnez à tous les utilisateurs les permissions de lecture et d'exécution sur le script.
3. Créer une entrée dans le fichier `sudoers` permettant à alice d'exécuter le script en tant que bob.
4. Valider qu'alice peut exécuter le script.
5. Utilisez `sudo` avec alice pour exécuter le script en tant que bob. Normalement, l'opération devrait échouer. Pourquoi?
