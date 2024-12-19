# TP SAMBA 
## création d'une nouvelle debian 
Dans l'interface vCenter,il faut sélectionnez l'option "Create a New Virtual Machine" et lui donner un nom *VM_D_Evan*
il faut choisir debian 11 (64 bits) et **selectionné linux** comme **systéme d'exploitation invité** puis il faut laisser par défaut les autres paramétres , il faut ensuite **modifier les paramétres** et mettre comme**périphérique fichier iso banque de donnée** et **forcé le démarrage** aprés il faut faire **remonter le cd room en premier pour démarrer l'ordinateur ,où se trouve l'iso**
ensuite il faut metrre un mot de passe et un identifiant.
il faut **désinstaller gnome pour avoir la debian sans interface graphique et l'environnement.** 
![](Images/1.png)
![](Images/2.png)
![](Images/3.png)
## Installation de samba
avant d'installer samba il faut installer les paquets **apt-get install,apt-get upgrade, apt-get install sudo**
![](Images/4.png)
![](Images/5.png)
aprés il faut faire sudo apt update puis il faut faire **sudo apt install samba -y** pour installer le serveur samba 
![](Images/6.png)
## Création du répertoire nommé partage dans le répertoire /srv/
tout d'abord il il faut utilisé la commande **sudo mkdir /srv/partage** pour créer le **répertoire partage dans /srv/**
pour voir si le répertoire partage à été créer il faut faire **ls**
![](Images/7.png)
## créer un groupe sambausers
pour créer un groupe sambausers,il faut faire **sudo groupadd sambausers**
![](Images/8.png)
## donner des droits au répertoire partage
Pour donner les droits 770 au répertoire /srv/partage il faut utiliser la commande **sudo chmod 770 /srv/partage**
les droits **770** signifie que **le propriétaire du répertoire a tous les droits (lecture, écriture, exécution)**,
**le groupe associé au répertoire a également tous les droits et les autres utilisateurs n'ont aucun droit d'accès**.
pour voir **vérifier les permissions** il faut faire **ls -ld /srv/partage**
cela doit nous afficher **drwxrwx--- 2 root sambausers 4096 Dec  5 14:33 /srv/partage**
![](Images/9.png)
## Associer groupe sambausers en propriétaires du répertoire /srv/partage
Pour associer le groupe sambausers en propriétaires du répertoire /srv/partage,
on doit utiliser la commande **sudo chown :sambausers /srv/partage**
pour voir la modification il faut faire **ls -ld /srv/partage**
![](Images/10.png)
## Créer des utilisateurs et  les affecter au groupe sambausers (p.nom et un professeur)
pour cela il faut **d'abord créer l'utilisateur p.nom** avec la commande **sudo useradd p.nom -m**
et il faut lui attribuer un mot de passe donc il faut faire la commande **sudo passwd p.nom**
![](Images/11.png)
Aprés il faut créer l'utilisateur professeur donc il faut faire **sudo useradd professeur -m**,
il faut lui définir un mot de passe avec la commande **sudo passwd professeur**
![](Images/12.png)
ensuite il faut les **ajouter au groupe sambausers**, t'abord il faut ajouter l'utilisateur "p.nom" au groupe sambausers
avec la commande **sudo usermod -aG sambausers p.nom**
aprés il faut ajouter l'utilisateur "professeur" au groupe sambausers avec la commande **sudo usermod -aG sambausers professeur**, on vérifie que les utilisateurs sont bien ajoutés au groupe avec la commande **groups p.nom** et **groups professeur**
![](Images/13.png)
## Associer les à la base samba grâce à la commande smbpasswd
Pour les associers à la base samba il faut **créer leurs comptes Samba avec la commande smbpasswd**
tout d'abord il faut ajouter l'utilisateur p.nom à la base Samba et lui attribuer un mot de passe Samba 
avec la commande **sudo smbpasswd -a p.nom**
aprés il faut ajouter l'utilisateur professeur à la base Samba et lui attribuer un mot de passe Samba 
avec la commande **sudo smbpasswd -a professeur**
![](Images/14.png)
Pour vérifier que les utilisateurs ont été correctement ajoutés à la base Samba,on utilise la commande suivante 
**sudo pdbedit -L**
![](Images/15.png)
## configurer le fichier de configuration smb.conf
il faut ouvrir le fichier de configuration de Samba avec un éditeur de texte en mode super utilisateur en 
fesant **sudo nano /etc/samba/smb.conf**
ensuite il faut ajoutez la section suivante à la fin du fichier smb.conf. Cela configurera le partage Partage
donc il faut faire **path = /srv/partage
valid users = @sambusers
read only = yes
write list = p.nom**
![](Images/16.png)
il faut faire ctrl + O pour sauvegarder et entrer pour quitter.
## Redémarrer le service smbd
Pour redémarrer le service smbd il faut faire **sudo systemctl restart smbd**
![](Images/17.png)

**Le fichier de configuration smb.conf de Samba contient de nombreuses options qui définissent le comportement du serveur Samba. Il permet de configurer les partages, la sécurité, les utilisateurs, les groupes et d'autres paramètres réseau.**

##  Sur la machine Windows connecter votre lecteur réseau et essayer de vousconnecter avec votre compte.
![](Images/18.png)
il faut aller dans l'explorateur de fichier puis cliquer sur les ... et cliquer sur connecter un lecteur réseau aprés 
dans dossier il faut mettre **\\172.26.104.3\partage** et mettre le nom d'utilisateur et le mot de passe 
![](Images/19.png)
## Tester les droits que vous avez en tant qu’utilisateur p.nom puis déconnecter le pour vous connecter en tant que professeur.
on vérifier les groupes de l'utilisateur avec la commande **groups p.nom** et les **droits d'accées avec id p.nom** et on vérifie les droits d'accées à un fichier
avec la commande **ls -l /srv/parage** , on peut voir qu'on à les **droits de lecture et d'écriture et d'éxecution.**
![](Images/20.png)
![](Images/21.png)

## Créer un nouveau partage nommé Travaux Pratique auquel seul l’utilisateurrofesseur à le droit d'écriture et de lecture et les autres ont qu’un droit de lecture sur ce partage.

Pour cela il faut créer le dossier pour le partage avec la commande **mkdir TravauxPratique**
et aprés il faut configurer les permissions du dossier 
il faut changer le propriétaire du dossier pour professeur avec la commande **chown professeur:professeur TravauxPratique**
 les permissions pour que seul l'utilisateur professeur puisse écrire dans le dossier, tandis que les autres peuvent seulement lire
 avec **chmod 755 TravauxPratique**
 ![](Images/22.png)

 ## TP Samba

 1.On clique avec le bouton droit sur la machine virtuelle Samba et on selectionne modifier les paramétres 
 puis on clique sur ajouter un nouveau périphérique et on selectionne disque dur .

 2.la commande fdisk -l vous pouvez voir les disques branchés sur la
machine. il faire su pour avoir les droits puis /sbin/fdisk -l 

![](Images/23.png)

3. la commande **gdisk n'est pas installer** pour cela ont fait **sudo apt install gdisk**
4. on fait d'abord gdisk /dev/sdb
● o pour créer une nouvelle table de partition GPT puis sur Entrée (confirmer
avec Y et Entrée)
● n pour créer une nouvelle partition. Pour le numéro de partition, laissez par
défaut.
● Pour le choix du First Sector, laissez le choix par défaut.
● Idem pour le last sector, laissez par défaut.
● Pour le code de la partition, entrez fd00 cela correspond à Linux RAID
● Appuyez sur w pour enregistrer les changements et quitter gdisk (confirmer
avec Y et Entrée)
![](Images/24.png)
ensuite on fait pareil pour sdc , sdd et sde
4. pour installer le paquet mdadm , il faut faire sudo apt install mdadm
5. pour vérifier que tout est bon via mdadm l'outil de gestion du RAID : mdadm -E
/dev/sd[b-e]
6. On va maintenant demander à mdadm de créer une grappe en lui précisant plusieurs
paramètres :
● /dev/mdX : correspondant au nom de la grappe
● --level=Y : indique que je souhaite créer un RAID Y
● --raid-devices=Z : indique le nombre de disques qui vont composer mon
RAID
● et ensuite on indique le nom des disques
Faire la commande suivante pour créer une grappe RAID 5 :
mdadm --create /dev/md0 --level=5 --raid-devices=4 /dev/sd[b-e]1
7. On peut contrôler l'avancement de la création de la grappe RAID avec la commande
suivante : cat /proc/mdstat
![](Images/25.png)
8.On va pouvoir créer un système de fichiers de types ext4 sur notre RAID :
mkfs.ext4 /dev/md0

9.Monter votre RAID sur le répertoire /mnt/monraid :
● mkdir /mnt/monraid
● mount /dev/md0 /mnt/monraid/

10. sudo nano /etc/fstab puis /dev/md0 /mnt/monraid ext4 defaults 0 0
11. exécuter ces trois commandes :
● cp /etc/mdadm/mdadm.conf /etc/mdadm/mdadm.conf.old
● mdadm --examine --scan --verbose >> /etc/mdadm/mdadm.conf
● update-initramfs -u -k all

13) Vérifier l’état des disques du RAID : mdadm --detail /dev/md0
14) pour supprimer un disque on fait clique droit sur la vm , modifier les paramétres
 et on supprime le dique que l'on veut .
15) 


