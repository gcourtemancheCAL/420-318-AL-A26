# Exercices ACL

## Important

- Vérifiez que les commandes `getfacl` et `setfacl` sont disponibles sur votre système. Si absent, vous aurez besoin de les installer. Ils sont normalement dans le package `acl`.
- Travaillez dans un répertoire de test (ex.: `/tmp/ex_acl` ou votre home) pour éviter de modifier des fichiers critiques.

---

## Mise en place de l'environnement

Créez les utilisateurs suivants :
1. `alice`
2. `bob`
3. `carl`
4. `dilbert`

Créez ensuite un groupe `projetx` et ajoutez `alice` et `bob` à ce groupe.

Créez ensuite un groupe `projety` et ajoutez `bob` et `carl` à ce groupe.

Questions :
1. Quelle est la différence entre permissions Unix classiques (UGO) et ACL?
2. Pourquoi les ACL sont utiles dans un environnement avec plusieurs équipes?

---

## Exercice 1 - Lecture des ACL existantes

1. Créez un dossier de travail `ex_acl` dans un répertoire disponible à tout le monde.
2. Créez un fichier `notes.txt` dans ce dossier.
3. Affichez :
	- les permissions classiques avec `ls -l`
	- les ACL avec `getfacl`

Questions :
1. Quelles entrées ACL voyez-vous par défaut?
2. Que représente l'entrée `mask`?
3. Pourquoi `ls -l` ne montre pas toutes les informations ACL?

---

## Exercice 2 - Donner un accès spécifique à un utilisateur

Objectif : permettre à `bob` de lire un fichier sans le rendre lisible à tout le monde.

1. Sur `notes.txt`, retirez les permissions pour `others`.
2. Vérifiez que `bob` ne peut pas lire le fichier.
3. Ajoutez ensuite une ACL pour que `bob` puisse lire le fichier.
4. Testez l'accès de `bob`.

Questions :
1. Quelle commande avez-vous utilisée pour accorder l'accès?
2. Quelle différence observe-t-on entre le mode `chmod` et l'ACL affichée par `getfacl`?
3. Quel symbole apparaît dans `ls -l` quand une ACL étendue est présente?

---

## Exercice 3 - ACL pour un groupe précis

Objectif : donner des droits au groupe `projetx` uniquement.

1. Créez un fichier `rapport.txt` appartenant à `alice` avec les permissions 660.
2. Accordez au groupe `projetx` les permissions de lecture et d'écriture via ACL.
3. Vérifiez qu'un membre du groupe (`bob`) peut modifier le fichier.
4. Vérifiez qu'un non-membre (`carl`) ne peut pas le modifier.

Questions :
6. Pourquoi ne pas simplement utiliser `chmod 664` dans ce cas?
7. Quel est l'avantage d'une ACL de groupe sur ce scénario?

---

## Exercice 4 - ACL pour un second groupe

1. Ajoutez au fichier `rapport.txt` les permissions en lecture au groupe `projety` via ACL.
2. Vérifiez que `carl` et `bob` puisse en lire le contenu. 
3. Est-ce que `bob` peut y écrire? Pourquoi?
4. Est-ce que `dilbert` peut lire le fichier?

---

## Exercice 5 - ACL par défaut sur un dossier

Objectif : appliquer automatiquement des ACL aux nouveaux fichiers.

1. Créez un dossier `partage` appartenant à `alice`.
2. Ajoutez une ACL par défaut pour que le groupe `projetx` ait `rw` sur les nouveaux fichiers.
3. Créez deux nouveaux fichiers dans `partage`.
4. Vérifiez leurs ACL.

Questions :
1. Quelle est la différence entre une ACL "normale" et une ACL "default"?
2. Les ACL par défaut s'appliquent-elles aux fichiers déjà existants?
3. Quel problème de collaboration ce mécanisme permet-il d'éviter?

---