# Exercices permissions

## Exercice 1 - Permissions sur les fichiers

1. Pour chacune des situations suivantes, indiquez les permissions minimales requises
	1. Un fichier `notes.txt` doit être lisible par tout le monde, modifiable seulement par son propriétaire, jamais exécutable.
	2. Un script `maintenance.sh` doit être exécutable seulement par son propriétaire, et illisible pour les autres.
	3. Un fichier `confidentiel.txt` doit être accessible uniquement à son propriétaire.
	4. Un fichier `journal.log` doit être lisible par le groupe `audit`, mais non modifiable par ce groupe.

2. Un fichier qui ne m'appartient pas est présent dans mon répertoire home. Il a des permissions de 000. Est-ce que je peux le supprimer? Pourquoi?

---

## Exercice 2 - Permissions sur les dossiers

Considérant l'arborescence suivante :

```text
/projet
  /secret
	 /docs
		plan.txt
```

1. Quelles permissions minimales faut-il sur `/projet`, `/secret` et `/docs` pour qu'un utilisateur puisse lire `plan.txt` avec `cat`, sans pouvoir lister le contenu de `/secret`?
2. Est-il possible de permettre `cd /projet/secret/docs` tout en empêchant `ls /projet/secret`? Quelles permissions?
3. Donnez un exemple de configuration de permissions (format symbolique ou octal) qui permet :
	- de traverser `/projet/secret`;
	- d'empêcher la création/suppression de fichiers dans `/projet/secret`;
	- de lire `plan.txt`.

---

## Exercice 3 - Observation des permissions de `/home`


1. Quelles sont les permissions du répertoire `/home` sur votre système?
2. Quelles sont les permissions du home de votre utilisateur sur votre système?
3. Souvent, le dossier `/home` dispose des permissions `755` tandis que les répertoires home des utilisateurs vont avoir les permissions `700`. Pourquoi? Quelle(s) permission(s) est-ce qu'on pourrait enlever du répertoire `/home`?  

---

## Exercice 4 - Investigation sur `passwd`

Contexte : un utilisateur non root peut changer son mot de passe avec `passwd`, alors que la base de données des mots de passe est protégée et requiert les accès root.

1. Formulez une hypothèse expliquant ce comportement.
2. Comparez les permissions des exécutables `passwd` et `find`
3. Identifiez la différence clé entre les deux permissions et expliquez sa signification.
4. Concluez : comment `passwd` réussit-il à écrire une ressource protégée par root?

---

## Exercice 5 - Trouver les exécutables SUID/SGID

1. Proposez une commande pour lister tous les exécutables SUID.
2. Proposez une commande pour lister tous les exécutables SGID.
3. Proposez une commande unique qui liste les deux catégories.

---

## Exercice 6 - Comportements du bit `x` sur les scripts

Créez un script `demo.sh` :

```bash
#!/bin/sh
echo "Bonjour depuis demo.sh"
```

1. Retirez le bit `x` et testez :
	- `./demo.sh`
	- `sh demo.sh`
2. Comparez les résultats et expliquez pourquoi ils diffèrent.
3. Ajoutez le bit `x` et répétez les tests. Expliquez vos résultats.
4. Expliquez le rôle du shebang (`#!/bin/sh`) dans ce contexte.
5. Retirez la permission en lecture et répétez les tests. Expliquez vos résultats.

---

## Exercice 7 - Investigation sur le sticky bit de `/tmp`

1. Identifiez les permissions du dossier `/tmp`. Que remarquez vous de particulier? Expliquez.
2. Donnez un scénario d'abus possible si le sticky bit était absent du dossier.

