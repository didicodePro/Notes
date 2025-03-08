Dyma git

git diff
Permet de comparer votre répertoire de travail (ou working directory) avec votre (zone de transit (ou staging area)
compare votre repertoire de travail avec votre zone de stage
git diff --staged
Cette commande compare votre zone de transit (votre index) avec votre dernier commit.
compare votre zone de transit avec votre dernier commit

Commit
=====

git commit -m "Second commit de test" -m "Nous avons ajouté deux lignes dans notre fichier test.txt afin de tester le comportement de Git lorsque nous modifions plusieurs lignes"
git commit -am "Suppression de la dernière ligne" -m "Nous supprimons la dernières ligne pour tester l'indexation automatique des fichiers suivis."

afficher les objets du repertoire git
find .git/objects -type f


Log
===

git log --author="dymafr"
git log --since=1.weeks
git log --stat
git log -p
git log --pretty=oneline
git log --grep Premier
git log -S 'Linus Torvald'

Blame
=====

git blame index.html
git help blame


La commande git clean permet de supprimer tous les fichiers non suivis du répertoire de travail, sauf ceux contenus dans .gitignore.
git clean -n
git clean -f
git clean -i

git revert
==========

Annuler un changement snas  creer un nouveau commit
git revert --no-commit commit
si on ne veut pas modifier le message
git revert commit --no-edit

git reset
=========

par default git reset par default retire les fichier de la zone de stage (Unstage)

git reset avec un commit
git reset --soft reviens mais conserve (Working directory) et (index(stage))
git reset --mixed fais un Unstage  default
fit reset --hard supprime tout (Working directory) et (index(stage))

git reset sans commit ne fera rien d'irrémediable :
Cela va simplement écraser l'index par la version pointée par le commit HEAD (autrement dit désindexer les modifications), on pourra toujours réindexer les modifications du répertoire de travail plus tard.


git branch
==========

git branch --merged
git branch --no-merged
git branch -d auth
git cat-file H (file= le nom du fichier, H =  Hash)

git merge --abort pour annuler le merge

Imaginons que vous travaillez en équipe et qu'un utilisateur utilise les tabulations et un autre des espaces dans VS Code pour le formatage du code.

Lorsque vous aurez une fusion, vous rencontrerez de nombreux conflits à cause de ces divergences entre espaces et tabulations.

Dans ce cas vous pouvez utiliser une option spécifique permettant d'ignorer ces différences pour la fusion :
git merge -X ignore-all-space

Comment fait Git pour vérifier les différences entre les trois sources ? Pour rappel les trois sources sont : la version du commit pointé par la branche où l'on se trouve, la version du commit pointé par la branche que l'on souhaite fusionner, et la version du commit qui est le premier ancêtre commun entre les deux branches.

Nous pouvons bien visualiser cela en faisant :
git ls-files -u
L'option -u pour --unmerged permet de montrer les fichiers non fusionnés, qui sont donc en conflit.


Pour privilégier toujours la branche à fusionner dans la branche actuelle, auth dans notre exemple, il faut faire :
git merge auth -X theirs
Si nous voulons au contraire privilégiez la version des fichiers de main lors des conflits, il faut faire :
git merge auth -X ours

Attention ! Il ne faut surtout pas confondre -X avec -s !!

Avec -X vous passez une option de fusion à la stratégie de fusion utilisée par défaut, ici la stratégie trois sources récursives.

Avec -s vous spécifiez la stratégie à adopter, autrement dit vous n'utilisez plus la stratégie récursive.


git stash
=========

git stash -u
permet de stasher les fichiers untracked
git stash -a
git stash list
git stash show
affiche les infos du stash
git stash show -p idStash
donne plus de detaille comme les ajouts et les suppression ex: git stash show -p stash@{0}
git stash branch Id(l'id du stash ex: stash@{0} Ex2: git stash branch feature stash@{0})
git stash clear supprimer tous les stash

Parfois vous voudrez sauvegarder temporairement les modifications dans votre répertoire de travail mais pas les modifications indexées
Par exemple si vous voulez faire un commit avec les modifications propres (que vous avez donc indexées) et ne sauvegarder temporairement que les modifications qui sont dans votre répertoire de travail pour terminer votre travail plus tard. Dans ce cas il faut faire :
git stash --keep-index

Comme nous l'avons vu, par défaut lors de l'application d'une sauvegarde temporaire, l'index n'est pas appliqué.
Pour l'appliquer il faut utiliser l'option --index avec l'une des deux commandes vu précédemment.
git stash apply --index

git rebase
==========

on se place sur un autre branch par exemple dev ensuit
git rebase main
il vas deplacer tout les commit de dev et les placer apres le dernier commit de main

-i
git rebase -i <start commit> <end commit> Ex: git rebase -i HEAD~3 concerne les 3 dernier commit
squash merge les deux commit

git merge squash
======================
git merge dev --squash 
le merge --squash est particulièrement utile quand il y a plusieurs commit fait rapidement avec des mauvais messages de validation
tres utilse

git reflog
==========

Le ReFlog est uniquement local, cela signifie que ce sont les déplacements que vous avez fait localement uniquement.
git reflog
git show HEAD@{2}
Cela affichera les détails du commit sur lequel était placé HEAD en avant-dernier.
Nous pouvons donc créer une branche avec un commit :
git branch recup 8ca8356

Nous pouvons faire une fusion en avance rapide sur main pour le récupérer sur la branche principale :
git merge recup
git branch -d recup

git pull
========

git pull --rebase permet d'éviter des commits de fusion (de merge)


git push
========

le git push fais un merge fast forward
Vous pouvez supprimer une branche distante en faisant :
git push --delete branche ou
git push origin :branche

git branch -vv
git branch -u origin/main pour creer le lien

Vous pourriez ainsi modifier le nom de la branche sur le dépôt distant, par exemple pour l'appeler authentification :
git push origin auth:authentification

prendre l'url ssh
git remote set-url origin git@gitlab.com:erwan.vallee/gittest.git

git prune
=========

git remote prune origin
git fetch --prune
L'option --prune va supprimer les branches de suivi dont les branches amonts associées n'existent plus sur le dépôt distant.

Si vous voulez que les branches de suivi soient automatiquement supprimées lorsque les branches amonts n'existent plus et que vous faîtes un git fetch ou un git prune, vous pouvez faire :
git config --global fetch.prune true

git prune -n  ou git prune --dry-run
L'option --dry-run ou -n permet d'afficher ce qui serait supprimé avec git prune sans le faire immédiatement.

git gc
La commande git gc (pour garbage collection) permet de procéder au nettoyage et à l'optimisation de sa base de données locale git.

tag
===

git tag nom
Vous pouvez également créer un tag sur un commit spécifique plus ancien :
git tag nom hash
Créer un tag annoté
git tag -a v1.0.1 -m 'Version 1.0.1 - auth fonctionnelle'
Visualiser les données d'un tag annoté
git show nomtag ==> git show v1.0.1
Lister les tags
git tag
Se positionner à la version référencée par un tag
git checkout nomtag ==> git checkout v1.0.2
Supprimer un tag
git tag -d nomtag
Modifier un tag
git tag -f v1.3 15027957251b64cf874c3557a0f3547bd83b3ff6
Publier vos tags
git push origin nomtag
Ou pour tous les push d'un coup :
git push origin --tags

alias
=====

git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status