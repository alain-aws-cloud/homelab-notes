# OverTheWire - Bandit

## Objectif
Apprendre les bases de la ligne de commande Linux à travers les niveaux du jeu Bandit.

## Environnement
- Machine : ThinkPad T480
- Connexion : SSH vers bandit.labs.overthewire.org (port 2220)

## Niveaux 0 à 3

| Commande | Rôle |
|----------|------|
| `ssh bandit0@bandit.labs.overthewire.org -p 2220` | Connexion SSH sur un port précis |
| `ls -la` | Lister tous les fichiers, y compris les cachés |
| `cat readme` | Afficher le contenu d'un fichier |
| `cat ./-` | Lire un fichier nommé `-` (sinon cat le prend pour une option) |
| `cat "nom avec espaces"` | Lire un fichier dont le nom contient des espaces |
| `cd inhere` | Se déplacer dans un dossier |

## Problèmes rencontrés
- Le fichier nommé `-` : `cat -` ne fonctionnait pas, il fallait préciser le chemin `./-`.
- (à compléter)

## Ce que j'ai retenu
- Les fichiers qui commencent par un point sont cachés, il faut `ls -a` pour les voir.


## Niveau 4 → 5

**Objectif** : trouver le seul fichier lisible par un humain parmi plusieurs fichiers dans `inhere`.

| Commande | Rôle |
|----------|------|
| `file ./*` | Identifier le type de tous les fichiers (texte ou binaire) |
| `cat ./-file07` | Lire un fichier dont le nom commence par `-` |
| `cat -- -file07` | Autre solution : `--` marque la fin des options |
| `reset` | Réparer le terminal s'il affiche des caractères bizarres |

**Problèmes rencontrés**
- `cat -file07` : erreur, le `-` est pris pour une option.
- `cat file07` : fichier introuvable, j'avais oublié le tiret dans le nom.
- Solution : `cat ./-file07`.

**Ce que j'ai retenu** : `file` permet de connaître le contenu d'un fichier sans l'ouvrir, pratique pour éviter d'afficher du binaire.

## Niveau 5 → 6

**Objectif** : trouver un fichier lisible, de 1033 octets, non exécutable, parmi 20 dossiers.

| Commande | Rôle |
|----------|------|
| `find . -type f -size 1033c ! -executable` | Chercher un fichier selon sa taille et ses droits |
| `ls -la maybehere*` | Lister tout ce qui commence par « maybehere » |

**Ce que j'ai retenu**
- `find` évite de chercher à la main : une ligne au lieu de 200.
- `-size 1033c` : le `c` signifie octets.
- Droits `rwx` : r = lecture, w = écriture, x = exécution ; un `-` = droit absent.

## Niveau 6 → 7

**Objectif** : trouver un fichier quelque part sur le serveur (propriétaire bandit7, groupe bandit6, 33 octets).

| Commande | Rôle |
|----------|------|
| `find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null` | Chercher sur tout le serveur en masquant les erreurs |

**Ce que j'ai retenu**
- `find /` cherche à partir de la racine, donc partout.
- `2>/dev/null` envoie les messages d'erreur (« Permission denied ») à la poubelle.
- Linux a deux sorties : la sortie normale (`>`) et les erreurs (`2>`).

- ## Niveau 7 → 8

**Objectif** : trouver le mot de passe à côté du mot « millionth » dans `data.txt`.

| Commande | Rôle |
|----------|------|
| `grep millionth data.txt` | Afficher seulement les lignes qui contiennent « millionth » |

**Ce que j'ai retenu**
- `grep` filtre un gros fichier pour ne garder que les lignes utiles.
- Options utiles : `-i` (ignore les majuscules), `-n` (numéro de ligne), `-v` (inverse).
- Cas réel : `grep "Failed password" /var/log/auth.log` pour repérer les connexions SSH ratées.

- ## Niveau 8 → 9

**Objectif** : trouver la seule ligne qui n'apparaît qu'une fois dans `data.txt`.

| Commande | Rôle |
|----------|------|
| `sort data.txt \| uniq -u` | Trier puis garder la ligne unique |
| `sort data.txt \| uniq -c \| sort -n` | Compter les occurrences de chaque ligne, puis trier par nombre |

**Ce que j'ai retenu**
- `uniq` ne compare que les lignes qui se suivent, donc il faut trier avant avec `sort`.
- Le pipe `|` envoie le résultat d'une commande à la suivante.
- Cas réel : compter les IP qui reviennent le plus dans un fichier de log.

- ## Niveau 9 → 10

**Objectif** : trouver le mot de passe dans un fichier binaire, juste après plusieurs `=`.

| Commande | Rôle |
|----------|------|
| `strings data.txt` | Extraire le texte lisible d'un fichier binaire |
| `strings data.txt \| grep "=="` | Garder seulement les lignes qui contiennent `==` |

**Ce que j'ai retenu**
- `cat` sur un binaire affiche des caractères illisibles ; `strings` ne garde que le texte.
- `grep` seul sur un binaire répond souvent « binary file matches » sans montrer la ligne.
- Cas réel : analyser un fichier suspect sans l'exécuter.

- ## Niveau 10 → 11

**Objectif** : décoder un fichier encodé en Base64.

| Commande | Rôle |
|----------|------|
| `base64 -d data.txt` | Décoder un fichier Base64 |
| `echo "texte" \| base64` | Encoder un texte en Base64 |

**Ce que j'ai retenu**
- Base64 est un encodage, pas un chiffrement : pas de clé, tout le monde peut le décoder.
- On le trouve dans les secrets Kubernetes et dans des scripts malveillants pour cacher des commandes.
