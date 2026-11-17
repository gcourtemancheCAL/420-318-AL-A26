# Chiffrement et hachage 

## Encodage des données

Les données informatique sont toutes encodées sous forme binaire. 

Par exemple, pour le texte, nous allons avoir plusieurs encodages permettant de représenter les différentes glyphes visuelles sous une forme binaire.

Plusieurs encodages existent pour le texte, tel que ASCII, UTF-8, UTF-16, EBCDIC, ...

**Exemple : ASCII**

<img src="img/Pasted image 20261030094112.png" width="600" />

Les données spécifiques aux différentes applications (sauvegarde de jeu, fichiers multimédia, etc) sont toutes encodées sous un format binaire qui leur est propre.

Ultimement, votre ordinaire va toujours et uniquement travailler avec des séquences binaires.

## Chiffrement

Le chiffrement consiste à prendre des informations immédiatement lisible et à les convertir sous un nouveau format codé dorénavant illisible.

Une opération de chiffrement se veut réversible uniquement si l'on dispose d'une information secrète.

Le chiffrement viens donc assurez que seul les acteurs disposant d'un secret puissent accéder aux données originales.  En d'autres mots, c'est un mécanisme par lequel on peut assurer la confidentialité des données 

Dans un contexte informatique, les données originales ainsi que les données chiffrées sont sous format binaire.

### Algorithmes de chiffrement

Un algorithme de chiffrement décrit la méthode par laquelle on encode et décode l'information. Plusieurs algorithmes de chiffrement ont été utilisés à travers les époques. 

Tous les algorithmes ne se valent pas.

#### Chiffrement par substitution

Méthode de chiffrement où les lettres dans un textes sont substitués par des lettres différentes.
Pour décoder le message, il faut connaitre les substitutions qui ont été faites.

##### Algorithme de Caesar

Chiffrement par substitution. Les substitutions sont obtenues en décalant l’alphabet d’un certain nombre de lettre.

**Exemple avec Caesar 4:**

<img src="img/Pasted image 20261030095827.png" width="600" />

##### ROT13

[À ne pas confondre avec ROTT13](https://en.wikipedia.org/wiki/Rise_of_the_Triad_(2013_video_game)

Cas spécial de l’algorithme de Caesar. Rotation de 13


<img src="img/Pasted image 20261030095951.png" width="600" />

#### Chiffrement par substitution

On peut aussi faire des substitutions arbitraires


<img src="img/Pasted image 20261030100029.png" width="600" />

### Chiffrement modernes

Les algorithmes modernes sont beaucoup plus complexes et mathématiquement éprouvés. Ils consistent d'une série d'opérations mathématiques effectuer sur une combinaison de données et clé.

Le résultat est facile à calculer lorsque nous disposons de la clé, mais très difficile à décoder sans la clé.

Les algorithmes modernes vont être classés en deux principales catégories : 
- Algorithmes symétrique
- Algorithmes asymétrique

**Exemple avec AES** 

<img src="img/Pasted image 20261030100231.png" width="600" />

#### Chiffrement symétrique

Un algorithme de chiffrement symétrique utilise le même mot de passe pour chiffrer et déchiffrer les données.

On va surtout préconiser ce genre d'algorithmes dans les contextes où on veut qu'un utilisateur choisisse/se rappelle d'un mot de passe. Le chiffrement est ruiné si la clé tombe entre des mauvaises mains.

Exemple de cas d'utilisation  : 
- archive 7zip protégé par un mot de passe
- chiffrement wifi

##### AES

AES est un algorithme de chiffrement symétrique commun

https://www.devglan.com/online-tools/aes-encryption-decryption

Séparent les données en blocs de tailles fixes et effectue une quantité de rondes de transformations mathématiques.

Vient en 3 variantes déterminants la taille de la clé (et donc des blocs) : 128,192, et 256 bits.

#### Encodage asymétrique

Un algorithme de chiffrement asymétrique utilise une clé différente pour le chiffrement que pour le déchiffrement.

Les clés doivent être mathématiquement reliées et doivent donc être générés.

Comme les opérations de chiffrement et de déchiffrement se font avec deux clés distinctes, il est possible de partager une de nos clés ce qui vient nous permettre de créer procédures plus complexes et plus intéressantes.

Nous allons généralement qualifier la clé que l'on partage de clé publique et l'autre de clé privée.

##### Communication sécurisée

<img src="img/Pasted image 20261030102725.png" width="600" />

##### Signature

<img src="img/Pasted image 20261030102738.png" width="600" />

## Hachage

Le hachage est une transformation mathématique qui prend une donnée d'entrée et retourne un hash de taille fixe.

Contrairement au chiffrement, le hachage est **non réversible**. On ne peut pas retrouver la donnée originale à partir du hash.

Un algorithme de hachage doit toujours produire le même output (le même hash) pour le même input (la même donnée).

Changer un seul bit d'information doit produire un résultat très différent.

### À quoi ça sert?

Le hachage est énormément utilisé en sécurité :
- stockage des mots de passe
- vérification d'intégrité de fichiers
- signatures numériques

### Exemple simple

Si on calcule SHA-256 pour deux textes presque identiques, le résultat est complètement différent.

Exemple :
- `Bonjour` -> `9172e8eec99f144f72eca9a568759580edadb2cfd154857f07e657569493bc44`
- `bonjour` -> `2cb4b1431b84ec15d35ed83bb927e27e8967d75f4bcd9cc4b25c8d879ae23e18`

### Algorithmes communs

- MD5 (rapide, mais pas sécuritaire)
- SHA-256 (encore très utilisé)
