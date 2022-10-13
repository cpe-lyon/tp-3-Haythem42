BELHADJ MANSOUR Haythem
3ICS - 2022/2023

# TP 3 - Utilisateurs, groupes et permissions

## Exercice 1. Gestion des utilisateurs et des groupes

1 - Pour créer les deux nouveaux avec la commmande **groupadd**, il a fallu utiliser les commandes ```sudo groupadd dev``` et ```sudo groupadd infra```. L'utilisation de **sudo** est nécessaire car nous n'avons pas les droits en tant qu'utilisateur.

2 - Pour créer un utilisateur avce la commande **useradd**, il faut utiliser les commandes ```sudo useradd nom``` et ce pour chaque nouvel utilisateur (alice, bob, charlie, dave). Le dossier se crée automatiquement en ajoutant l'option **-m**. Par défaut les utilisateurs ont pour shell l'interpreteur sh (on peut le vérifier en faisant ```cat /etc/passwd```). Pour leur attribuer le shell **bash**, il faut utiliser la commande **usermod** de cette façon : ```sudo usermod nom -s /bin/bash```. Pour vérifier que le shell a bien été modifié, on peut taper la commande ```cat /etc/passwd```.

3 - Pour ajouter un utilisateur dans un groupe secondaire (créé précédement), on utilise la commande ```sudo usermod -a -G nom_groupe nom_utilisateur```. On met ainsi alice, bob et dave dans le groupe *dev* et bob, charlie et dave dans le groupe *infra*.

4 - Pour afficher les membres d'un groupe, on peut utiliser la commande ```grep nom_groupe /etc/group```. Ainsi, pour afficher les membres du groupe infra, on peut faire ```grep infra /etc/group```. Une autre méthode serait d'afficher la liste des groupes avec ```cat /etc/group``` et de chercher le groupe qui nous intéresse.

5 - Pour modifier le groupe propriétaire d'un répertoire, on peut utiliser la commande ```sudo chgrp -R nom_groupe cible```. On fait cette commande pour **/hom/alice** et **/home/bob** comme cible avec le groupe **dev**. De même pour **/home/charlie** et **/home/dave** avec le groupe **infra**.

6 - Pour remplacer/modifier le groupe primaire d'un utilisateur, on peut utiliser la commande ```sudo usermod utilisateur -g groupe```. Ex. : sudo usermod alice -g dev. On fait ça pour **bob** qui est aussi dans le groupe **dev** et **charlie** et **dave** pour le groupe **infra**.

7 - Pour créer les deux répertoires /home/dev et /home/infra, on se rend dans /home avec la commande ```cd /home``` puis on utilise la commande ```sudo mkdir dev infra```. Une fois ces deux répertoires créés, on va donner les permissions d'écriture (write) aux membres des groupes respectifs en faisant la commande ```sudo chmod g+w dev infra```. On peut vérifier que les permissions ont bien été administrées avec la commande ```ls -l```, un **w** devrait être présent pour les *group*.

8 - Pour que seul le propriétaire d'un fichier ait le droit de renommer ou supprimer ce fichier, on met en place un sticky bit, pour se faire, on utilise la commande ```sudo chmod +t nom_dossier```.

9 - Nous ne pouvons pas ouvrir de session en tant qu'Alice car le compte n'a pas de mot de passe, il est donc inactif.

10 - Pour activer le compte d'un utilisateur, il faut lui définir son mot de passe avec la commande ```sudo passwd nom_utilisateur```. On pourra ainsi donner le "nouveau" mot de passe pour le compte utilisateur. Pour alice, nous utilisons la commande ```sudo passwd alice```. On doit taper deux fois le nouveau mot de passe et un message devrait appaaître : "passwd : password updated successfully" indiquant que le mot de passe a bien été enregistré. On peut maintenant se connecter avec le compte d'alice avec la commande ```su alice```et en tapant le nouveau mot de passe. On se retrouve ainsi sur son compte.

11 - Une fois connecté sur son compte, il nous suffit d'utiliser la commande ```id``` pour obtenir l'uid (=1002) et le gid (=1002) d'alice. En dehors de son compte, on peut utiliser la commande ```id alice``` pour trouver ces informations.

12 - En sachant que l'uid est placé à la troisième position dans le fichier /etc/passwd, on peut simplement utiliser la commande ```grep 1003 /etc/passwd```. Ici l'utilisateur en question est bob.

13 - Alice et Bob faisant parti du groupe dev, on peut se référer au gid trouvé en tapant la commande ```id alice``` ou ```ìd``` (si on est connecté en tant qu'alice). L'id du groupe dev est 1002.
Autre possibilité plus simple : utiliser la commande ```grep dev: /etc/group```, on remarque bien que l'id du group est 1002.

14 - Le groupe **dev** a l'id 1002. (on emploie la même méthode que précédemment).

15 - Pour retirer un utilisateur d'un groupe, on tape la commande ```gpasswd -d utilisateur groupe```. Ici, on souhaite supprimer l'utilisateur **charlie** du groupe **infra**. On tape donc la commande ```gpasswd -d charlie infra```. D'après le système, **charlie** a bien été retiré du groupe. On peut vérifier en tapant la commande ```grep infra /etc/group```, on remarque qu'il ne fait plus partie. Cependant, lorsqu'on tape ```id charlie```, on voit que son groupe primaire reste l'**infra**. Nous ne sommes pas sensé pouvoir supprimer un utilisateur de son groupe (s'il n'en a qu'un).

16 - Pour modifier les informations de compte de **dave** (se trouvant dans /etc/shadow), on utilise ces commandes :
 - Expiration le 1e juin 2021 : usermod --expiredate 2021-06-01 dave
 - Changer le mot de passe avant 90 jours : ```passwd --maxdays 90 dave```
 - Attendre 5 jours pour modifier un mot de passe : ```passwd --mindays 5 dave```
 - Avertissement 14 jours avant l'expiration du mot de passe : ```passwd --warndays 14 dave```
 - Blocage du compte 30 jours après expiration du mot de passe : ```passwd --inactive 30 dave```

17 - L'intérpréteur de commande de l'utilisateur **root** est le bash. La commande pour trouver cette information est ```grep root /etc/passwd```.

18 - On peut retrouver le compte nobody avec la commande ```grep nobody /etc/passwd```. *nobody* est le nom conventionnel d'un compte utilisateur à qui aucun fichier n'appartient. Il n'est dans aucun groupe qui possède des privilèges et a dont les seules possibilités sont celles que tous les "autres utilisateurs" ont (il aura les droit de **o**).

19 - La commande **sudo** conserve le mot de passe en mémoire pendant 15 minutes.
La commande ```sudo -k``` permet de forcer **sudo** à oublier le mot de passe.


# Exercice 2

1 - Lorsqu'on vient de simplement créer un dossier et un fichier dans ce dernier, les droits de **test** (le dossier) sont 775. Il a tous les droits sauf celui d'écrire (write) dans **other**. Quant à **fichier** (le fichier), ses droits sont 664. Cela correspond aux droits de lire et écrire dans **user** et **group**, et seulement le droit de lire dans **other**.

2 - Pour retirer tous les droits du fichier, on utilise la commeande ```chmod a-rw fichier```. On peut vérifier que toutes les permissions soient bien retiré, puis on peut tenter d'afficher et/ou de modifier le fichier, mais on remarque assez vite qu'on ne peut plus. En tentant cette fois ci en tant que root, on remarque qu'on peut lire et modifier le fichier. On peut en conclure que le **root** dispose de toutes les permissions quoi qu'il arrive.

3 - Pour se redonner les droits d'écriture et d'exécution, on utilise la commande ```chmod u+wx fichier```. Les droits permettent à l'utilisateur d'écriredans le fichier, il ne pourra cependant pas lire le contenu du fichier car il en a pas les droits.

4 - Pour exécuter le fichier, on tape la commande ```./fichier```, cependant le système nous indique que nous n'avons pas les droits. En utilisant **sudo**, le fichier s'exécute correctement et *Hello* s'affiche. C'est dû au fait qu'on ait pas les droits de lecture, on ne peut donc pas avoir accès au contenu du fichier.

5 - Pour nous retirer les droits de lecture du répertoire, nous utilisons la commande ```chmod u-r ../test/```. En tentant de lister le contenu du répertoire avec la commande ```ls```, le système nous indique que nous n'avons pas les droits. On ne peut pas non plus lire ou exécuter le fichier **fichier**. On en déduit que le fait de ne pas avoir le droit de lecture dans le répertoire nous empêche de manipuler les fichiers présents dans le répertoire. Pour rétablir le droit en lecture sur **test**, on tape la commande ```chmod u+r ../test/```.

6 - Pour retirer les droits d'écriture, on tape les commandes ```chmod a-w nouveau``` et ```chmod a-w ../test/```. On ne peut pas écrire dans le fichier **nouveau**. On ne peut pas modifier le fichier **nouveau** puisqu'on a précédemment retiré les droits d'écriture sur ce fcihier. En voulant le supprimer, le système nous demande si l'on souhaite vraiment supprimer le fichier protégé en écriture, en tapant **yes**, le fichier est supprimé. On peut en déduire que les droits que nous possédons par rapport au dossier priment sur les droits des fichiers présents dans ce répertoire. La manipulation des fichiers d'un répertoire dépendent d'abord des droits que l'utilisateur possède sur le dossier, puis des droits sur les fichiers que l'on souhaite manipuler.

7 - On ne peut ni créer, ni modifier, ni supprimer de fichier dans le répertoire **test**. On ne peut pas nonplus s'y déplacer, cependant on peut lister son contenu mais pas nous n'avons pas accès aux fichiers et dossiers présents dans **test**. On peut en déduire que la permission d'exécution joue un rôle important car sans ce droit, nous ne pouvons en aucun cas manipuler le dossier et son contenu.

8 - Une fois dans le répertoire **test**, si on enlève les droits d'exécution, nous ne pouvons plus rien faire dans celui-ci. Les droits sont importants, notamment les droits d'exécution car sans ces droits nous sommes limités dans notre manipulation de fichier/dossier. Et sans le droit d'exécution, nous ne pouvons rien faire. On peut effectivement retourner dans le répertoire courant avec ```cd ..``` car cela ne requiert pas les droits du dossier **test** mais plutôt nos droits sur le répertoire courant. En l'occurance, nous possédons les droits d'écriture, de lecture et d'exécution dans notre répertoire courant.

 9 - Après avoir rétabli le droit d'exécution sur le répertoire **test** avec la commande ```chmod u+x ../test/```, nous utilisons la commande ```chmod g+r fichier``` pour donner le droit d'accès en lecture à un membre du groupe.
 
 10 - On va utiliser la commande ```umask 077```afin de restreindre au maximum les droits du **group** et de **other**. En faisant ll pour pour le nouveau fichier et le nouveau dossier, on peut remarquer que seul l'utilisateur dispose de droits sur ces 2 fichiers. **group** et **other** n'ont auncun droits. On peut également vérifier grâce à la commande ```umask -S``` qui nous indique les droits pour chacun. Ici, le système nous retourne : ```u=rwx,g=,o=```.
 
 11 - On va utiliser la commande ```umask 022```afin de permettre à **group** et **other** de pouvoir lire les fichiers de l'utilisateur et de pouvoir traverser les répertoires. En faisant ll pour pour le nouveau fichier et le nouveau dossier, on peut remarquer que le **group** et **other** peuvent lire et exécuter (r=read, x=execute). Ils n'ont cependant pas le droit d'écriture (w=write qui est ici dans cet état : **-**). On peut également vérifier grâce à la commande ```umask -S``` qui nous indique les droits pour chacun. Ici, le système nous retourne : ```u=rwx,g=rx,o=rx```.
 
 12 - On va utiliser la commande ```umask 037``` afin de nous autoriser tous les droits et de permettre aux membres de mon groupe un accès en lecture aux fichier et répertoires. En faisant ll pour pour le nouveau fichier et le nouveau dossier, on peut remarquer que l'utilisateur peut lire et écrire, et que les membres du groupe peuvent seulement lire, que ce soit pour le nouveau fichier ou le nouveau dossier. On peut également vérifier grâce à la commande ```umask -S``` qui nous indique les droits pour chacun. Ici, le système nous retourne : ```u=rwx,g=r,o=```.
 
 13 - On transcrit chaque commande de la notation classique à la notation octale et vice-versa :
 - ```chmod u=rx,g=wx,o=r fic``` -> ```chmod 534 fic```
 - ```chmod uo+w,g-rx fic``` -> ```chmod 602 fic```
 - ```chmod 653 fic``` -> ```chmod u-x,g+r,o+w fic```
 - ```chmod u+x,g=w,o-r fic``` -> ```chmod 520 fic```

14 - En tapant la commande ```ll /etc/passwd```, on remarque que seul les droits d'écriture et de lecture sont accordés à l'utilisateur. Les membres du groupes ont, eux, le droit de lecture. De plus, le proprétaire est le root. Seul le root est amené à devoir modifier le programme pour éviter qu'un membre ne le modifie. Il est donc normal que seul le propriétaire ai les droits d'écriture. Les autres n'ont pas de droits sur le programme car celui-ci contient les informations concernant les utilisateurs de la machine.
