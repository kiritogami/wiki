# Travailler avec Git

*Dernière mise à jour : {{ git_revision_date_localized }}*

## 1. Comment déposer un projet sur GitHub/GitLab

### Étape 1 : Initialisation
* Le projet est dans un dossier ; on ouvre le terminal bien sûr dans le dossier et on initialise avec un dépôt git : `git init`

---

### Étape 2 : Committer les fichiers
* La première chose à faire c'est de pré-ranger les fichiers à déposer : dans le premier dépôt nous on veut déposer tous le dossier donc on indique **"."** pour préciser que c'est le dossier courant qui contient tous les fichiers. On tape la commande suivante : `git add .`
* Après on enregistre ce qu'on a préparé avant (on fait ça avec la commande commit) et on ajoute un message : `git commit -m "Initial commit"`

---

### Étape 3 : Création du dépôt distant
* Sur GitHub/GitLab je dois créer un dossier ; et après j'obtiendrai un URL de projet
* Après je dois ajouter un dépôt distant pour créer le lien entre mon PC et GitHub/GitLab avec la commande : `git remote add <NAME> <URL>`
    * **Name :** C'est juste un alias de l'URL pour ne pas le répéter dans tous les commandes.
* Pour vérifier les dépôts distants déjà configurés on tape : `git remote -v`
* Pour changer l'URL d'un dépôt qui existe déjà : `git remote set-url <NAME> <New URL>`

---

### Étape 4 : Push ton code
* Avant de pousser le code je dois créer un branch sur lequel je pousse ; on fait ça avec la commande : `git branch <nom branch>`
* Maintenant on doit envoyer nos commits qui sont local avec la commande : `git push -u <name of distant depot> <branche envoyée>`

---

## Questions

### Questions Générales

* Si j'ai poussé un code dans une branche est-ce que je peux annulé cette action ?
* supposons que j'ai commité un fichier 1 et après je l'ai pas pousser mais j'ai commité un autre fichier 2 est-ce que lorsque je vais pousser ça va être le fichier 1 ou le premier ?

### Réponses

* **Quand on pousse :** ce sont tous les fichiers qui n'ont pas été pousser qui le vont être ; donc c'est le premier et le deuxième.
    * Avec la commande : `git log alias/branche_name` on peut voir les commits qui n'ont pas été encore poussés.

### Afficher les Branches

* Comment on peut voir les branches qui existent déjà ?
    * Avec la commande : `git branch`