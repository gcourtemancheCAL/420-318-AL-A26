# Permissions UNIX

Le modèle de permissions Unix repose sur trois droits (**RWX**) appliqués à trois catégories d'identités (**UGO**). C'est le mécanisme de contrôle d'accès de base sur tout système Unix/Linux.

Les permissions sont contenus dans l'attribut *st_mode* de la structure *stat* d'un fichier. Cet attribut est un nombre entier sur 16 bits. Les droits des différents catégories d'utilisateurs y sont contenus sous forme de bitmask - pour chaque catégorie d'utilisateur, 3 bits sont utilisés pour enregistrer les permissions en lecture, écriture et d'exécution.

3 bits sont utilisés pour représenter la présence d'attributs spéciaux. 

Les 4 bits restants identifient le type de fichier (e.g. dossier, fichier régulier, lien symbolique, ...)


```
stat.st_mode = 0000 000 000 000 000b
			   TYPE FLG USR GRP OTH
			            RWX RWX RWX
```
 
## RWX

Les trois droits fondamentaux sont **r**ead, **w**rite et e**x**ecute. Leur effet concret diffère selon qu'ils s'appliquent à un fichier ou à un dossier.

### Impacts sur les fichiers

| Droit | Lettre | Valeur | Effet sur un fichier |
|-------|--------|--------|----------------------|
| Lecture   | `r` | 4 | Lire le contenu du fichier |
| Écriture  | `w` | 2 | Modifier (ajouter, remplacer ou tronquer) le contenu du fichier |
| Exécution | `x` | 1 | Lancer le fichier comme programme ou script |

**Exemple :** un script `backup.sh` avec `rw-------` peut être lu et modifié par son propriétaire, mais ni exécuté ni accessible par les autres. Pour l'exécuter, il faut ajouter `x` : `chmod u+x backup.sh`.

**Attention :** la permission en écriture sur un fichier ne permet pas de le supprimer — la suppression dépend des droits du **dossier** qui le contient.

**Attention :** la permission en exécution a des limitations particulières et non intuitives.

### Impacts sur les dossiers

| Droit | Lettre | Valeur | Effet sur un dossier |
|-------|--------|--------|----------------------|
| Lecture   | `r` | 4 | Lister les fichiers du dossier (`ls`) |
| Écriture  | `w` | 2 | Créer, renommer ou supprimer des fichiers dans le dossier |
| Exécution | `x` | 1 | Traverser / entrer dans le dossier (`cd`) |

**Exemple :** un dossier `projets/` avec `r-x` pour le groupe permet aux membres de lister et d'entrer dans le dossier, mais pas d'y créer ou supprimer des fichiers.

**Cas particulier :** `r` sans `x` sur un dossier permet de voir les noms de fichiers mais pas d'y accéder. `x` sans `r` permet de traverser le dossier si on connaît le nom exact du fichier, sans pouvoir lister son contenu.

## UGO

Chaque fichier possède exactement un **propriétaire** et un **groupe** propriétaire. Les permissions sont définies indépendamment pour trois catégories :

- **u (user)** : le propriétaire du fichier.
- **g (group)** : les utilisateurs membres du groupe propriétaire.
- **o (others)** : tous les autres utilisateurs du système.

Les 9 bits de permission sont affichés sous forme de trois triplets `rwx` dans la sortie de `ls -l` :

```
-rwxr-x---  1  alice  devs  4096  ...  script.sh
 ^^^---^^^
  u  g  o
```

La **notation octale** exprime chaque triplet par un chiffre (r=4, w=2, x=1) :

| Symbolique  | Octal | Description |
|-------------|-------|-------------|
| `rwxrwxrwx` | `777` | Accès total pour tout le monde (à éviter) |
| `rwxr-xr-x` | `755` | Standard pour les exécutables/dossiers publics |
| `rw-r--r--` | `644` | Standard pour les fichiers lisibles publiquement |
| `rw-------` | `600` | Fichier privé (clés SSH, configs sensibles) |
| `rwx------` | `700` | Script ou dossier strictement privé |

**Commandes principales :**

```bash
ls -l fichier.txt                  # afficher les permissions
chmod 750 script.sh                # notation octale
chmod u+x,g-w fichier.txt         # notation symbolique
chown alice:devs fichier.txt       # changer propriétaire et groupe
chown -R www-data /var/www         # récursif
```

## Flags spéciaux

En plus des 9 bits rwx, trois flags spéciaux modifient des comportements particuliers.

**Setuid (SUID) :** un exécutable avec SUID s'exécute avec les droits du **propriétaire** du fichier, pas ceux de l'utilisateur qui le lance.

```bash
chmod u+s /usr/bin/xyzzy   # activer SUID (apparaît comme 's' à la place de 'x')
```

**Setgid (SGID) :** sur un exécutable, il s'exécute avec les droits du groupe propriétaire. Sur un **dossier**, tout nouveau fichier créé à l'intérieur hérite automatiquement du groupe du dossier.

```bash
chmod g+s /partage/equipe/   # activer SGID sur un dossier (apparaît comme 's')
```

- **Risque :** les binaires SUID et SGID root sont des cibles privilégiées pour l'escalade de privilèges. D'autres mécanismes sont à privilégier lorsque possible.

---

**Sticky bit — bit 1 :** sur un dossier, seul le **propriétaire d'un fichier** peut le supprimer, même si d'autres ont le droit `w` sur le dossier.

```bash
chmod +t /tmp   # activer le sticky bit (apparaît comme 't' à la place de 'x' sur others)
```

## Limitations

Le modèle UGO présente plusieurs limitations importantes pour des environnements complexes :

- **Granularité binaire :** un fichier n'a qu'un seul propriétaire et un seul groupe. Il est impossible d'accorder des droits différents à des utilisateurs ou groupes distincts sur le même fichier sans mécanismes supplémentaires. Par exemple, on ne peut pas dire "alice peut lire, bob peut lire et écrire" avec le modèle de base.

- **Pas de contrôle sur les capacités réseau ou système :** le modèle UGO ne permet pas de limiter finement ce qu'un processus peut faire au niveau du noyau.

Ces limitations ont conduit au développement de mécanismes complémentaires : **sudo**, **capabilities** et **ACL**.

---

# Systèmes supplémentaires

## Sudo

`sudo` (substitute user do) permet à un utilisateur d'exécuter une commande avec les privilèges d'un autre utilisateur (typiquement root), selon des règles définies dans `/etc/sudoers`.

Une bonne configuration `sudo` peut remplacer l'utilisation des bits SUID et SGID sur des fichiers exécutables. 

```bash
sudo commande                    # exécuter en tant que root
sudo -u alice commande           # exécuter en tant qu'alice
sudo -l                          # lister ses propres droits sudo
```

La configuration dans `/etc/sudoers`  définit précisément qui peut faire quoi :

```
# Syntaxe : user  host=(usr_dst:grp_dst)  commandes
# Le host est utile lorsqu'on veut réutiliser le même fichier sur plusieurs 
# systèmes. Typiquement, on va le voir comme ALL. 
alice   ALL=(ALL)       ALL              # alice peut tout faire en sudo
bob     ALL=(root)      /usr/bin/apt     # bob ne peut lancer que apt

# L'exemple ici permet a tous les membres du groupe devs de lancer cette commande 
# __sans avoir à fournir de mot de passe__.
%devs   ALL=(root)      NOPASSWD: /bin/systemctl restart nginx
```

**IMPORTANT :**  Toujours utiliser la commande `visudo` pour modifier le fichier sudoers. On peut préciser l'éditeur de texte à utiliser en spécifiant la variable EDITOR : `EDITOR=/bin/vim visudo` 

**Exemple :** un administrateur accorde à un développeur le droit de redémarrer uniquement le service `nginx` sans lui donner un accès root complet.

**Risque :** `sudo ALL` est équivalent à un accès root. Une mauvaise configuration sudoers est un vecteur classique d'escalade de privilèges.

```bash
sudo -l   # première commande lors d'un test d'intrusion pour identifier les sudo exploitables
```

[Plus d'information sur sudo](https://wiki.archlinux.org/title/Sudo)

[Réflexions sur accès root et sudo](https://www.digitalocean.com/community/tutorials/how-to-edit-the-sudoers-file)

**Voir les logs sudo :** ``journalctl /usr/bin/sudo``

## Capabilities

Les **capabilities** Linux découpent les privilèges root en unités indépendantes et granulaires. Plutôt que d'accorder un SUID root complet, on accorde uniquement la capacité nécessaire.

Les *capabilities* sont généralements assignées directement aux fichiers exécutables. Ainsi, on peut rendre disponibles certains comportements sensibles spécifiques aux utilisateurs sans avoir à instaurer une politique complexe de permissions au niveau des utilisateurs.

Un exemple d'application utilisant les *capabilities* serait `fping` qui dispose de `cap_net_raw` lui permettant de créer des sockets de type `SOCK_RAW` sans disposer de permissions root.

[En savoir plus sur les capabilities sous linux](https://wiki.archlinux.org/title/Capabilities)

## ACL

Les **ACL** (Access Control Lists) étendent le modèle UGO en permettant de définir des permissions pour des utilisateurs ou groupes supplémentaires sur un fichier ou dossier.

```bash
# Afficher les ACL d'un fichier
getfacl fichier.txt

# Ajouter un droit de lecture pour l'utilisateur bob
setfacl -m u:bob:r fichier.txt

# Ajouter des droits pour le groupe audit
setfacl -m g:audit:rx /var/log/app/

# ACL par défaut sur un dossier (héritées par les nouveaux fichiers)
setfacl -d -m g:devs:rw /partage/projet/

# Supprimer une entrée ACL
setfacl -x u:bob fichier.txt

# Supprimer toutes les ACL
setfacl -b fichier.txt
```

La présence d'ACL sur un fichier est signalée par un `+` à la fin des permissions dans `ls -l` :

```
-rw-r--r--+  1  alice  devs  ...  fichier.txt
```

**Exemple :** un dossier de logs appartient à `root:root` avec les permissions `700`. Les ACL permettent d'accorder un accès en lecture seule à l'utilisateur `monitoring` sans modifier la propriété ni créer un nouveau groupe.

```bash
setfacl -m u:monitoring:r /var/log/app/
```

**Limitation :** les ACL doivent être supportées par le système de fichiers (ext4, xfs les supportent nativement). Elles ne sont pas toujours préservées lors de copies ou archives si on n'utilise pas les bonnes options (`cp -a`, `tar --acls`).

[Précédence des règles ACL](https://unix.stackexchange.com/a/353629)