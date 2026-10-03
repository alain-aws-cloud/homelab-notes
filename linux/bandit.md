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
- (à compléter)
