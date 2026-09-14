
# Consignes : 

En petite équipe, analysez la situation qui vous est donnée et préparez une présentation sur le sujet. Vous devrez faire une démonstration utilisant les outils vus en classe durant votre présentation.

# Mises en situation :

## Situation 1 - Compte partagé au laboratoire

Dans un laboratoire informatique, plusieurs étudiants utilisent le même compte `eleve` pour se connecter par ssh à un serveur. Le compte est partagé afin de simplifier la gestion des permissions.

À couvrir dans la présentation :
- Résumez la situation.
- Quels principes de sécurité sont brisés?
- Quels éléments de la triade CIA sont affectés? Comment? 
- Parlez de l'identification, l'authentification et l'autorisation dans ce contexte.
- Proposez une alternative plus sécuritaire à l'aide des outils vus en classe et faites en une démo.

## Situation 2 - Dossier RH mal configuré

Le dossier réseau `RH` appartenant à `rh:rh` contient des informations sensibles sur les différents employés. Afin d'assurer l'accès aux gestionnaires des différents départements et aux employés des ressources humaines, le dossier est mis disponible pour tout le monde.

À couvrir dans la présentation :
- Résumez la situation.
- Quels principes de sécurité sont brisés?
- Quels éléments de la triade CIA sont affectés? Comment? 
- Parlez de l'identification, l'authentification et l'autorisation dans ce contexte.
- Proposez une alternative plus sécuritaire à l'aide des outils vus en classe et faites en une démo.

## Situation 3 - Installation d'applications

Des postes informatiques Linux sont configurés de façon générique pour les développeurs d'une compagnie. L'entreprise autorise les développeurs à installer n'importe quelle application parmi une liste prédéterminée. Dans cet objectif, tous les développeurs se font donner des paramètres de connexion root.

À couvrir dans la présentation :
- Résumez la situation.
- Quels principes de sécurité sont brisés?
- De quel genre de liste d'accès est-ce qu'on parle? Quel principe de sécurité est-ce que ça concerne? 
- Parlez de l'identification, l'authentification et l'autorisation dans ce contexte.
- Proposez une alternative plus sécuritaire à l'aide des outils vus en classe et faites en une démo.

## Situation 4 - Session admin déjà connectée

Un technicien laisse son portable ouvert sans surveillance avec un terminal ouvert sur lequel une session SSH connectée en root est établie vers l'un des serveurs de l'entreprise.

À couvrir dans la présentation :
- Résumez la situation.
- Quels principes de sécurité sont brisés?
- Quels éléments de la triade CIA sont affectés? Comment? 
- Parler de l'identification, l'authentification et l'autorisation dans ce contexte.
- Faites une démonstration de comment une bonne utilisation de sudo pourrait permettre une application du principe "confiance zéro" dans ce contexte.

## Situation 5 - Répertoire partagé

Dans un laboratoire, tous les étudiants utilisent un dossier partagé pour déposer leurs travaux. Le dossier est disponible en lecture, écriture et exécution pour tout le monde. Aucun flag spécial n'y est assigné.

Les étudiants peuvent y déposer leurs fichiers via `scp` en utilisant un compte personnel propre à chaque étudiant.

À couvrir dans la présentation :
- Résumez la situation.
- Quels principes de sécurité sont brisés?
- Quels éléments de la triade CIA sont affectés? Comment? 
- Parler de l'identification, l'authentification et l'autorisation dans ce contexte.
- Proposez une alternative plus sécuritaire à l'aide des outils vus en classe et faites-en une démo.
