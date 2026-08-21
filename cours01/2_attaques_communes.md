# Attaques communes

## Logiciels malveillants

Un logiciel malveillant (malware) est un programme conçu pour endommager un système, voler des données, espionner l'utilisateur ou prendre le contrôle d'un appareil.

### Classification par mode de propagation

#### Virus

Code malveillant qui s'attache a un fichier ou programme légitime et s'active lorsque ce fichier est exécuté.

**Exemple:** un fichier `setup.exe` modifié infecte d'autres exécutables sur l'ordinateur.

La définition d'un virus concerne spécifiquement le mode de propagation. Cependant, il est très souvent utilisé à tort comme terme générique pour parler d'un logiciel malveillant.

#### Cheval de Troie

Programme qui se présente comme légitime pour tromper la victime, sans mécanisme automatique de réplication.

**Exemple:** Les serveurs de mise à jour de Notepad++ ont été compromis en 2025 et livrait une version compromise de l'utilitaires aux utilisateurs.

#### Vers

Malware autonome qui se propage seul, souvent via le réseau, en exploitant des vulnérabilites.

**Exemple:** WannaCry est un ver connu qui infectait automatiquement les systèmes Windows pas à jour rejoignable sur le réseau. WannaCry était bâtit sur la base de l'exploit EternalBlue.

### Classification par comportement

Ce qui suit est loin d'être une liste exhaustive - l'objectif ici n'est que de donner quelques exemples.

#### Ransomware

Malware qui chiffre les fichiers puis exige un paiement pour en restaurer l'accès.

**Exemple:** WannaCry était un ransomware qui chiffrait les fichiers sur un système avant de demander une rançon.

#### Infostealer

Malware spécialisé dans le vol d'informations: mots de passe, cookies de session, données bancaires, portefeuilles crypto.

#### RAT

RAT (Remote Access Tool): malware qui offre un accès distant complet a l'attaquant.

**Exemple:** l'attaquant active la webcam a distance et navigue dans les fichiers de la victime.

## Ingénierie Sociale

Technique de manipulation psychologique qui pousse une personne a divulguer une information, cliquer sur un lien ou contourner une procédure de sécurité.

**Exemple:** un faux courriel du "service TI" demande de réinitialiser le mot de passe sur un site frauduleux.

## D/DoS

- **DoS (Denial of Service):** une seule source surcharge une ressource pour la rendre indisponible.
- **DDoS (Distributed Denial of Service):** plusieurs machines (souvent un botnet) attaquent simultanément.

**Exemple:** un site web de commerce en ligne devient inaccessible pendant une promotion a cause d'un trafic malveillant massif.