BELHADJ MANSOUR Haythem
3ICS - 2022/2023

# TP 3 - Utilisateurs, groupes et permissions

## Exercice 1. Gestion des utilisateurs et des groupes

1. Pour créer les deux nouveaux avec la commmande **groupadd**, il a fallu utiliser les commandes ```sudo groupadd dev``` et ```sudo groupadd infra```. L'utilisation de **sudo** est nécessaire car nous n'avons pas les droits en tant qu'utilisateur.

2. Pour créer un utilisateur avce la commande **useradd**, il faut utiliser les commandes ```sudo useradd nom``` et ce pour chaque nouvel utilisateur (alice, bob, charlie, dave). Le dossier se crée automatiquement en ajoutant l'option **-m**. Par défaut les utilisateurs ont pour shell l'interpreteur sh (on peut le vérifier en faisant ```cat /etc/passwd```). Pour leur attribuer le shell **bash**, il faut utiliser la commande **usermod** de cette façon : ```sudo usermod nom -s /bin/bash```. Pour vérifier que le shell a bien été modifié, on peut taper la commande ```cat /etc/passwd```.

3. Pour ajouter un utilisateur dans un groupe secondaire (créé précédement), on utilise la commande ```sudo usermod -a -G nom_groupe nom_utilisateur```. On met ainsi alice, bob et dave dans le groupe *dev* et bob, charlie et dave dans le groupe *infra*.

4. Pour afficher les membres d'un groupe, on peut utiliser la commande ```grep nom_groupe /etc/group```. Ainsi, pour afficher les membres du groupe infra, on peut faire ```grep infra /etc/group```. Une autre méthode serait d'afficher la liste des groupes avec ```cat /etc/group``` et de chercher le groupe qui nous intéresse.

5. Pour modifier le groupe propriétaire d'un répertoire, on peut utiliser la commande ```sudo chgrp -R nom_groupe cible```. On fait cette commande pour **/hom/alice** et **/home/bob** comme cible avec le groupe **dev**. De même pour **/home/charlie** et **/home/dave** avec le groupe **infra**.

6. Pour remplacer/modifier le groupe primaire d'un utilisateur, on peut utiliser la commande ```sudo usermod utilisateur -g groupe```. Ex. : sudo usermod alice -g dev. On fait ça pour **bob** qui est aussi dans le groupe **dev** et **charlie** et **dave** pour le groupe **infra**.

7. Pour créer les deux répertoires /home/dev et /home/infra, on se rend dans /home avec la commande ```cd /home``` puis on utilise la commande ```sudo mkdir dev infra```. Une fois ces deux répertoires créés, on va donner les permissions d'écriture (write) aux membres des groupes respectifs en faisant la commande ```sudo chmod g+w dev infra```. On peut vérifier que les permissions ont bien été administrées avec la commande ```ls -l```, un **w** devrait être présent pour les *group*.

8. Pour que seul le propriétaire d'un fichier ait le droit de renommer ou supprimer ce fichier, on met en place un sticky bit, pour se faire, on utilise la commande ```sudo chmod +t nom_dossier```.

9. Nous ne pouvons pas ouvrir de session en tant qu'Alice car le compte n'a pas de mot de passe, il est donc inactif.

10. Pour activer le compte d'un utilisateur, il faut lui définir son mot de passe avec la commande ```sudo passwd nom_utilisateur```. On pourra ainsi donner le "nouveau" mot de passe pour le compte utilisateur. Pour alice, nous utilisons la commande ```sudo passwd alice```. On doit taper deux fois le nouveau mot de passe et un message devrait appaaître : "passwd : password updated successfully" indiquant que le mot de passe a bien été enregistré. On peut maintenant se connecter avec le compte d'alice avec la commande ```su alice```et en tapant le nouveau mot de passe. On se retrouve ainsi sur son compte.

11. Une fois connecté sur son compte, il nous suffit d'utiliser la commande ```id``` pour obtenir l'uid (=1002) et le gid (=1002) d'alice. En dehors de son compte, on peut utiliser la commande ```id alice``` pour trouver ces informations.

12. En sachant que l'uid est placé à la troisième position dans le fichier /etc/passwd, on peut simplement utiliser la commande ```grep 1003 /etc/passwd```. Ici l'utilisateur en question est bob.

13. Alice et Bob faisant parti du groupe dev, on peut se référer au gid trouvé en tapant la commande ```id alice``` ou ```ìd``` (si on est connecté en tant qu'alice). L'id du groupe dev est 1002.
Autre possibilité plus simple : utiliser la commande ```grep dev: /etc/group```, on remarque bien que l'id du group est 1002.

14. Le groupe **dev** a l'id 1002. (on emploie la même méthode que précédemment).

15.

16.

17. L'intérpréteur de commande de l'utilisateur **root** est le bash.
