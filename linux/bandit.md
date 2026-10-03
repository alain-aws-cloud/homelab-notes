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
