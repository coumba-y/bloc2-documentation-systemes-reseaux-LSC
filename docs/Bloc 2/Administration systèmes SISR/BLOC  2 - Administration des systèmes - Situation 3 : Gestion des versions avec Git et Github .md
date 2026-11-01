# BLOC  2 - Administration des systèmes - Situation 3 : Gestion des versions avec Git et Github 

**Compte rendu rédigé par** : BIDANESSY Coumba
**Formation** : BTS SIO 2ème année - Option SISR  
**Établissement** : Lycée Paul-Louis Courier, Tours  

[toc]




# TP Git / GitHub : Questions et réponses

## Partie 1 : Installation et configuration de Git

**Question 1 : Relever la version de Git installée.**
Réponse : `git version 2.55.0.5` 

**Question 2 : Quel est le rôle d'un logiciel de gestion de versions tel que Git ?**
Il enregistre l'historique des modifications d'un ensemble de fichiers. Il permet de revenir à une version antérieure, de savoir qui a modifié quoi et quand, de comparer des versions, de travailler à plusieurs sans écraser le travail des autres et d'assurer la traçabilité d'un projet.

**Question 3 : Pourquoi Git associe-t-il un nom et une adresse électronique aux commits ?**
Pour identifier l'auteur de chaque modification. Cela garantit la traçabilité, permet de contacter l'auteur en cas de problème et d'attribuer correctement le travail. GitHub utilise aussi l'adresse e-mail pour relier les commits à un compte.

---

## Partie 2 : Création d'un premier dépôt Git

**Question 4 : Que signifie l'initialisation d'un dépôt Git ?**
`git init` crée un dossier caché `.git` contenant toute la base de données Git (historique, branches, configuration). Le dossier devient alors un dépôt local dont Git peut suivre les fichiers.

**Question 5 : Quel est le rôle de la commande « git status » ?**
 Elle affiche l'état du dépôt : branche courante, fichiers non suivis, fichiers modifiés non indexés, fichiers indexés prêts à être commités.

---

## Partie 3 : Création des premières versions du script

**Question 6 : Quelle différence observez-vous dans « git status » avant et après la commande « git add » ?**
Avant, le fichier est dans *Untracked files* (non suivi). Après, il apparaît dans *Changes to be committed* (mention `new file`) : il est dans la zone d'indexation, prêt à être commité.

**Question 7 : Qu'est-ce qu'un commit ?**
Un instantané de l'état des fichiers indexés, enregistré définitivement dans l'historique du dépôt local, avec un identifiant unique (hash), un auteur, une date et un message.

**Question 8 : Pourquoi faut-il utiliser un message de commit précis et explicite ?**
Pour comprendre ce qui a été modifié et pourquoi, retrouver rapidement une modification dans l'historique, faciliter le travail en équipe et le dépannage.

---

## Partie 4 : Modification et suivi d'un fichier

**Question 9 : Que permet d'afficher la commande « git diff » ?**
Les différences ligne par ligne entre le fichier de travail et la dernière version indexée ou commitée. Les lignes ajoutées sont précédées de `+`, les lignes supprimées de `-`.

**Question 10 : Combien de versions du projet sont maintenant présentes dans l'historique ?**
2 versions (2 commits) : la création du script, puis l'ajout du test TCP/IP local.

---

## Partie 5 : Comprendre le fonctionnement de Git

**Question 11 : Indiquer le rôle de chacune des commandes : git status, git diff, git add, git commit et git log.**

| Commande | Rôle |
|---|---|
| `git status` | Affiche l'état du dépôt (fichiers non suivis, modifiés, indexés). |
| `git diff` | Montre les différences entre le fichier de travail et la dernière version indexée ou commitée. |
| `git add` | Ajoute les modifications à la zone d'indexation. |
| `git commit` | Enregistre définitivement le contenu de l'index dans le dépôt local. |
| `git log` | Affiche l'historique des commits (hash, auteur, date, message). |

---

## Partie 6 : Annulation d'une modification non validée

**Question 12 : Quel est l'intérêt de la commande « git restore » ?**
Elle annule les modifications non validées d'un fichier et le ramène à son dernier état enregistré. Utile pour corriger une erreur ou abandonner un essai sans toucher à la version stable.

**Question 13 : La modification supprimée avait-elle déjà été enregistrée dans un commit ? Justifier.**
Non. La ligne `Write-Host "TEST INCORRECT"` n'avait été ni indexée (`git add`) ni commitée (`git commit`) : elle n'existait que dans le répertoire de travail, ce qui a permis à `git restore` de la supprimer.

---

## Partie 7 : Découverte de GitHub

**Question 14 : Peut-on utiliser Git sans GitHub ? Justifier.**
Oui. Git est un outil local, indépendant de tout service en ligne. GitHub n'est qu'une plateforme d'hébergement distant qui s'ajoute à Git.

**Question 15 : Quelle différence faites-vous entre Git et GitHub ?**
Git est le logiciel de gestion de versions installé sur le poste. GitHub est un service web qui héberge des dépôts Git et ajoute le partage, la collaboration, la gestion des accès et les Pull Requests.

**Question 16 : Quelle différence existe-t-il entre un dépôt public et un dépôt privé ?**
Un dépôt public est visible et clonable par tout le monde sur Internet. Un dépôt privé n'est visible que par le propriétaire et les personnes qu'il autorise.

**Question 17 : Pourquoi un administrateur systèmes doit-il être vigilant avant de publier un script sur un dépôt public ?**
Un script peut contenir des informations sensibles (mots de passe, jetons, noms de serveurs, adresses IP) exploitables par des attaquants. De plus, l'historique Git conserve les anciennes versions : une donnée supprimée plus tard reste récupérable.

**Question 18 : Donner trois exemples d'informations qui ne doivent pas être publiées dans un dépôt GitHub.**

1. Mots de passe et identifiants de comptes.
2. Clés d'API, jetons d'accès et clés privées (SSH, certificats).
3. Informations internes sur l'infrastructure (adresses IP, noms de serveurs, plan d'adressage).

---

## Partie 8 : Relier le dépôt local au dépôt GitHub

**Question 19 : Que représente le nom « origin » ?**
C'est l'alias (nom court) du dépôt distant, par convention celui vers lequel on pousse ou d'où l'on a cloné. Il évite de retaper l'URL complète.

**Question 20 : Quelle commande permet d'envoyer les commits locaux vers GitHub ?**
 `git push` (ici `git push -u origin main` la première fois).

**Question 21 : Quelle différence existe-t-il entre « git commit » et « git push » ?**
`git commit` enregistre une version dans le dépôt local. `git push` envoie les commits locaux vers le dépôt distant (GitHub).

---

## Partie 9 : Modification et synchronisation avec GitHub

**Question 22 : Avant d'exécuter « git push », cette modification est-elle déjà visible sur GitHub ? Pourquoi ?**
 Non. Le commit n'existe que dans le dépôt local ; GitHub ne reçoit que ce qui est envoyé par `git push`.

---

## Partie 10 : Récupération et mise à jour d'un dépôt

**Question 23 : Quelle différence faites-vous entre « git init » et « git clone » ?**
`git init` crée un nouveau dépôt vide dans un dossier local. `git clone` copie un dépôt distant existant avec tout son historique et configure automatiquement `origin`.

**Question 24 : Pourquoi est-il important de récupérer les dernières modifications avant de commencer à travailler sur un projet partagé ?**
Pour partir de la version la plus récente, éviter de travailler sur une base obsolète, et prévenir les conflits ou le refus d'un `git push` dû à un dépôt distant ayant évolué.

---

## Partie 11 : Extension : travail avec des branches

**Question 25 : Comment Git indique-t-il la branche actuellement utilisée ?**
Par un astérisque `*` devant le nom de la branche dans `git branch`. `git status` l'indique aussi (`On branch ...`), ainsi que `(HEAD -> ...)` dans `git log`.

**Question 26 : Quel est l'intérêt de travailler dans une branche plutôt que directement dans « main » ?**
Développer une évolution de façon isolée sans risquer de casser la version stable, permettre un travail en parallèle, pouvoir abandonner un essai, et ne fusionner qu'après validation.

---

## Partie 12 : Modifier le projet dans une branche

**Question 27 : La modification réalisée dans « feature-diagnostic-dns » est-elle présente dans « main » ? Pourquoi ?**
Non. Le commit appartient uniquement à la branche `feature-diagnostic-dns`. Tant qu'elle n'est pas fusionnée, `main` ne le contient pas.

---

## Partie 13 : Fusion d'une branche

**Question 28 : Quel est le rôle de la commande « git merge » ?**
Elle intègre dans la branche courante les commits d'une autre branche, en réunissant deux lignes de développement (fast-forward ou commit de fusion).

---

## Partie 14 : Publier une branche et utiliser une Pull Request

**Question 29 : La branche apparaît-elle maintenant sur GitHub ?**
Oui, grâce à `git push -u origin feature-informations-systeme`. Elle est visible dans la liste des branches et GitHub propose « Compare & pull request ».

**Question 30 : Quelle différence existe-t-il entre une branche uniquement locale et une branche publiée sur GitHub ?**
Une branche locale n'existe que sur le poste (ni sauvegardée, ni visible par les autres). Une branche publiée existe aussi sur `origin` : elle est sauvegardée, partageable et utilisable pour une Pull Request.

**Question 31 : Quel est l'intérêt d'une Pull Request ?**
Proposer l'intégration d'une branche dans une autre de façon encadrée : visualiser les changements, les commenter, les faire relire et valider avant fusion, avec une trace de la discussion.

**Question 32 : Pourquoi est-il préférable de faire vérifier une modification avant son intégration dans « main » ?**
Pour garder `main` stable, détecter erreurs, mauvaises pratiques ou fuites d'informations sensibles avant la version de référence, et valider le travail en équipe.

---

## Partie 15 : Extension : comprendre un conflit de fusion

**Question 33 : Pourquoi Git ne choisit-il pas automatiquement une des deux versions ?**
Les deux branches ont modifié la même ligne. Git ne peut pas deviner l'intention des auteurs (garder l'une, l'autre ou une combinaison) et risquerait d'écraser un travail légitime.

**Question 34 : Qui doit décider du contenu à conserver ?**
L'administrateur qui réalise la fusion, éventuellement avec l'auteur de l'autre modification. Il édite le fichier, supprime les marqueurs `<<<<<<<`, `=======`, `>>>>>>>` et garde le contenu voulu.

**Question 35 : Après correction manuelle du conflit, quelles opérations Git permettent d'enregistrer la résolution ?**
Réponse :
```bash
git add diagnostic-reseau.ps1   # marque le conflit comme résolu
git commit                      # crée le commit de fusion
```

---

## Partie 16 : Synthèse

**36. Compléter le rôle des commandes suivantes :**

| Commande | Rôle |
|---|---|
| `git init` | Initialise un nouveau dépôt Git local dans le dossier courant. |
| `git status` | Affiche l'état du dépôt (branche, fichiers modifiés, indexés ou non suivis). |
| `git diff` | Affiche les différences entre le fichier de travail et la dernière version indexée ou commitée. |
| `git add` | Ajoute les modifications à la zone d'indexation avant le commit. |
| `git commit` | Enregistre les modifications indexées dans le dépôt local avec un message. |
| `git log --oneline` | Affiche l'historique des commits en une ligne par commit. |
| `git restore` | Annule les modifications non validées d'un fichier. |
| `git clone` | Copie un dépôt distant sur le poste avec tout son historique. |
| `git pull` | Récupère les commits du dépôt distant et les fusionne dans la branche locale. |
| `git push` | Envoie les commits locaux vers le dépôt distant. |
| `git branch` | Liste, crée ou renomme des branches (la branche active est marquée `*`). |
| `git switch` | Change de branche (`-c` pour créer puis basculer). |
| `git merge` | Fusionne les commits d'une autre branche dans la branche courante. |

**37. Expliquer en quelques lignes la différence entre Git, GitHub, un commit, une branche, une fusion et une Pull Request.**
- **Git** : logiciel local de gestion de versions décentralisé.
- **GitHub** : plateforme web d'hébergement de dépôts Git, avec partage, collaboration et Pull Requests.
- **Commit** : instantané enregistré de l'état des fichiers, avec auteur, date et message.
- **Branche** : ligne de développement parallèle et isolée.
- **Fusion** : intégration des modifications d'une branche dans une autre.
- **Pull Request** : demande, sur GitHub, d'intégrer une branche dans une autre avec relecture et validation.

**38. Un administrateur exécute « git add . » puis « git commit », mais oublie « git push ». Les autres administrateurs peuvent-ils voir ce commit sur GitHub ? Justifier.**
Non. Le commit n'est enregistré que dans son dépôt local. GitHub ne reçoit que ce qui est envoyé par `git push` : sans cette commande, le travail n'est ni partagé ni sauvegardé à distance.

**39. Un administrateur doit développer une fonctionnalité qui n'est pas encore validée. Doit-il modifier directement « main » ou créer une branche dédiée ? Justifier.**
 Il doit créer une branche dédiée (`git switch -c feature-...`). `main` reste ainsi stable ; on peut tester ou abandonner la fonctionnalité sans risque, la faire relire via une Pull Request et travailler en parallèle avec d'autres personnes.