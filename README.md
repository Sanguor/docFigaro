# Github workflow

## Structure

Deux branches principales :

- master
- develop

La branche "master" représente la production, un produit fonctionnel.

La branche "develop" représente le coeur du développement.

Trois types de branches contextuelles :

- feature
- release
- hotfix

Les branches de fonctionnalité ou "feature" sont utilisées pour développer de nouvelles fonctionnalités qui seront amenées à rejoindre "develop".

Les branches de version ou "release" sont utilisées pour définir une version et la mettre en production (sur "master").

Les branches de maintenance ou "hotfix" sont utilisées pour appliquer rapidement des patchs aux versions de production (sur "master").

![ ](https://wac-cdn.atlassian.com/dam/jcr:61ccc620-5249-4338-be66-94d563f2843c/05%20(2).svg?cdnVersion=1511 "Github workflow")

## Fonctionnement

### Feature

La branche doit être basée sur "develop" et être au format "feature/domaine/nom-de-la-feature/id-jira".

Les domaines possibles sont actuellement :

- unity
- server
- ai

Spécifier le domaine permet au pipeline, lors d'un push, de déclencher les contrôles adaptés.

Exemple pour une feature qui concerne le serveur :

Se placer dans "develop" et s'assurer d'être à jour
```git
git checkout develop
git pull
```

Créer la branche en local
```git
git checkout -b feature/server/feature-lambda/JOC-15
```

Créer la branche en remote
```git
git push --set-upstream origin feature/server/feature-lambda/JOC-15
```

---

Développer la feature ainsi que ses tests.

---

Une fois la feature terminée, récupérer la dernière version de "develop" pour l'intégrer à "feature/server/feature-lambda/JOC-15" (rebase)
```git
git pull --rebase origin develop
```

> Si des **conflits** apparaissent, les **résoudres** puis :
```git
  git add .
  git rebase --continue
  git push --force origin feature/server/feature-lambda/JOC-15
```

Faire sa pull request sur Github

### Release

La branche doit être basée sur "develop" au format "release/numéro.de.release".

> Le format du numéro de release est le suivant : ```Major.Minor.Hotfix``` (exemple: ```0.1.0```)

Se placer dans "develop" et s'assurer d'être à jour
```git
git checkout develop
git pull
```

Créer la branche en local
```git
git checkout -b release/0.1.0
```

Créer la branche en remote
```git
git push --set-upstream origin release/0.1.0
```

---

Review de la release, ajustements et/ou corrections si nécessaire.

---

Une fois la release prête, récupérer la dernière version de "master" pour l'intégrer à "release/0.1.0" (rebase)
```git
git pull --rebase origin master
```

> Si des **conflits** apparaissent, les **résoudres** puis :
```git
  git add .
  git rebase --continue
  git push --force origin release/0.1.0
```

Faire sa pull request sur Github pour "master".

Une fois la pull request validée et intégrée à "master", récupérer la dernière version de "develop" pour l'intégrer à "release/0.1.0" (rebase)
```git
git pull --rebase origin develop
```

> Si des **conflits** apparaissent, les **résoudres** puis :
```git
  git add .
  git rebase --continue
  git push --force origin release/0.1.0
```

Faire sa pull request sur Github pour "develop".

### Hotfix

La branche doit être basée sur "master" au format "hotfix/id-jira"

Se placer dans "master" et s'assurer d'être à jour
```git
git checkout master
git pull
```

Créer la branche en local
```git
git checkout -b hotfix/JOC-42
```

Créer la branche en remote
```git
git push --set-upstream origin hotfix/JOC-42
```

---

Développer le hotfix (+ tests si nécessaire).

---

Une fois le hotfix prêt, récupérer la dernière version de "master" pour l'intégrer à "hotfix/JOC-42" (rebase)
```git
git pull --rebase origin master
```

> Si des **conflits** apparaissent, les **résoudres** puis :
```git
  git add .
  git rebase --continue
  git push --force origin hotfix/JOC-42
```

Faire sa pull request sur Github pour "master".

Une fois la pull request validée et intégrée à "master", récupérer la dernière version de "develop" pour l'intégrer à "hotfix/JOC-42" (rebase)
```git
git pull --rebase origin develop
```

> Si des **conflits** apparaissent, les **résoudres** puis :
```git
  git add .
  git rebase --continue
  git push --force origin hotfix/JOC-42
```

Faire sa pull request sur Github pour "develop".

### Bug

La branche doit être basée sur "develop" au format "bug/id-jira".

> Penser à bien documenter le ticket Jira et à l'associer à la feature en défaut.

Se placer dans "develop" et s'assurer d'être à jour
```git
git checkout develop
git pull
```

Créer la branche en local
```git
git checkout -b bug/JOC-84
```

Créer la branche en remote
```git
git push --set-upstream origin bug/JOC-84
```

---

Développer le correctif ainsi que ses tests + update des autres tests au besoin.

---

Une fois le bug corrigé, récupérer la dernière version de "develop" pour l'intégrer à "bug/JOC-84" (rebase)
```git
git pull --rebase origin develop
```

> Si des **conflits** apparaissent, les **résoudres** puis :
```git
  git add .
  git rebase --continue
  git push --force origin bug/JOC-84
```

Faire sa pull request sur Github

### Notes

- Bien s'assurer d'être à jour avec la branche depuis laquelle on souhaite en créer une nouvelle !

- Une fois la(ou les) pull request acceptée, supprimer la branche locale avec :

```git
git branch -D nom-de-la-branche
```

- En cas de pull request successives (plusieurs personnes font une pull request), la première sera traitée et, si validée, la ou les pull request suivantes devront être modifiées fonction des nouvelles données présentes sur la branche cible.

---

### Todo

- Faisabilité des tests sur pull request à déterminer
