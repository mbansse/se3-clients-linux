# Memo Git

## Installation

Installation du minimum pour faire du git en ligne de commandes :

```sh
apt-get update
apt-get install openssl ca-certificates git
```

## Configuration du fichier `~/.gitconfig`

Quelques paramétrage à effectuer pour avoir de la couleur dans la sortie
des commandes (c'est juste indispensable la couleur) :

```sh
git config --global color.diff auto
git config --global color.status auto
git config --global color.branch auto
```

Les commandes ci-dessus ne sont qu'un moyen de modifier le
fichier (texte) de configuration `~/.gitconfig` (fichier de
configuration dans le home de utilisateur).

Il est important de bien paramétrer les éléments ci-dessous,
notamment l'adresse mail qui doit coïncider avec son adresse
mail sur GitHub :

```sh
git config --global user.name <son-login-github>
git config --global user.email <son-adresse-github>
```

## Commandes utiles

### Clonage d'un dépôt distant

En général avec GitHub, on crée un dépôt via l'interface Web de
GitHub et ensuite on clone ce dépôt en local sur sa (ses) machine(s)
personnelle(s). Dans le cas qui nous intéresse, le dépôt
`se3-clients-linux` existe déjà, il ne reste plus qu'à le cloner sur
sa machine. On se rend quelque part dans son home où on va placer un
répertoire qui contiendra le dépôt puis :

```sh
git clone git@github.com:flaf/se3-clients-linux.git
```

À partir de là, on a un répertoire `se3-clients-linux` qui vient de se
créer au niveau du répertoire courant et qui contient tout le projet (en
local donc). Le jour où on veut tout abandonner, on supprime le dépôt
local comme ceci (ça n'aura absolument aucune incidence sur le dépôt
distant, ie le dépôt sur GitHub) :

```sh
rm -rf se3-clients-linux/
```

### Update dépôt local

À faire régulièrement. Avec cette commande, on récupère toutes les
modifications (on appelle ça des commits) que les autres membres du
projet ont éventuellement poussées sur le dépôt distant. Bref, cela
permet d'avoir un dépôt local à jour par rapport au dépôt distant
(pull = tirer en anglais) :

```sh
git pull
```

### Vérification des modifications locales et commit

Imaginons qu'on ait **modifié** un fichier ou plusieurs fichiers
du projet. « Modifié » signifie qu'on a édité (avec son éditeur
favori, par exemple vim) un fichier ou plusieurs fichiers déjà
existants dans le projet. La commande :

```sh
git status
```

affichera la liste des fichiers modifiés sur le dépôt local et
qui n'ont pas encore été commités, ie validés en quelques sortes.
Pour commiter (ie valider) la liste des modifications inventoriées
par la commande `git status`, faire :

```sh
git commit -av
```

L'éditeur par défaut va s'ouvrir et il faudra indiquer un message
court expliquant nos modifications. Cela va commiter, ie valider
les modifications sur notre dépôt **local** et sur **lui seulement**
(à ce stade les modifications sont validées sur notre dépôt local mais
pas sur le dépôt distant).

**Remarque :** on peut parfaitement faire plusieurs modifications sur
N fichiers et regrouper tout cela en un seul commit. Il est toutefois
recommandé de faire des commits les plus atomiques possibles (ie une
modification, un commit, une modification un commit etc). En revanche,
on n'est pas obligé de pusher chaque commit les uns après les autres.
On peut parfaitement faire N commits (en local donc) et pusher nos N
commits en une fois.

### Remonter les modifications sur le dépôt distant

Imaginons qu'on ait effectué N commits. Comme on a vu ci-dessus, ces
N commits ne sont toujours pas remontés sur le dépôt distant, ie le
dépôt GitHub (et donc concrètement vous n'avez toujours pas partagé
vos commits avec les autres membres du projet). Pour pousser ces N
commits sur le site distant, on lance la commande :

```sh
# push = pousser.
git push
```

Pas de bonnes pratiques particulières au niveau des pushs. On peut très
bien faire un push après chaque commit mais ce n'est pas obligatoire.
On peut très bien faire 20 commits et pusher ensuite, peu importe. En
revanche, les commits, eux, doivent être le plus atomiques possibles :

```sh
# Une modification.
vim ./un/fichier

# Un commit.
git commit -av


# Une autre modification.
vim ./un/autre/fichier

# Un autre commit.
git commit -av

# Etc.

# Et à la fin, on pushe nos commits.
# On pourrait tout aussi bien pusher après chaque commit (personnellement,
# c'est ce que je fais).
git push
```

### Ajout de fichiers/répertoires au projet

Enfin, tout ce qui touche à la structure du projet doit passer
par des commandes git dédiées. Par exemple, pour **ajouter** un
nouveau fichier au projet :

```sh
# On crée le fichier dans notre dépôt local.
touch le-nouveau-fichier
```
ce qui crée localement un nouveau fichier mais pour l'instant ce
fichier ne fait pas encore parti du projet. D'ailleurs, la
commande :

```sh
git status
```

va indiquer que le nouveau fichier est `untracked`, ie qu'il ne
fait pas partie (encore) du projet. Pour l'intégrer au projet :

```sh
git add le-nouveau-fichier
```

Voici un autre exemple :

```sh
# Ajout d'un nouveau répertoire contenant un fichier.
mkdir ltsp
vim ltsp/le-fichier.ext
```

Là aussi, à ce stade, le fichier ne fait pas partie du projet. On
aura beau commiter ce qu'on veux, le fichier ne sera pas mis sur
GitHub. Il faut d'abord l'intégrer au projet avec :

```sh
git add ltsp/le-fichier.ext
```

On vient alors d'ajouter le fichier au projet.
Git comprendra qu'il faut aussi rajouter le répertoire `ltsp` du coup.
On peut à nouveau éditer le fichier etc. puis :

```sh
vim ltsp/le-fichier.ext
git commit -av
git push
```

### Le renommage d'un fichier/répertoire

Le renommage modifie modifie la structure d'un projet. Il possède lui
aussi une commande dédiée :

```sh
# Renomme le fichier1 en fichier2
git mv ./fichier1 ./fichier2
git commit -av
git push
```

### Manipulation de branches

```sh
# Ces commandes supposent que les branches existent déjà soit en local
# soit sur le dépôt distant (ie GitHub) :
git checkout master    # bascule dans la branche master
git checkout test-truc # bascule dans la branche test-truc
```

Lorsqu'on change de branche, l'arborescence locale va se modifier
automatiquement afin de coller à la branche choisie. Il n'y aura pas
2 arborescences différentes, juste une seule qui collera automatiquement
à la branche choisie (la « magie » est dans le répertoire `.git/` qui se
trouve à la racine du dépôt local et qui contient toutes les informations
nécessaires afin que git se débrouille pour que l'arborescence locale soit
en phase avec la branche sur laquelle on se trouve).


# Memo sur le formatage markdown (fichiers `.md`)

En fait, tout est parfaitement résumé
[ici](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet).

* Pour faire un titre : `# Titre`
* Pour faire un sous titre : `## Sous titre`
* Pour faire un sous sous titre : `### Sous sous titre`
* Pour mettre en gras  : `**du gras**`
* Pour faire une liste à puce : ̀`*`
* Pour faire un lien : `[cette page](<url relative ou absolue>)`

Pour insérer du code sur une ligne de texte ou dans d'un bloc,
tout est expliqué
[ici](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet#code-and-syntax-highlighting).

