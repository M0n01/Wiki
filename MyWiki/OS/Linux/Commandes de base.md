![[TUX.png|500]]
# Raccourci clavier

CTRL + R    // rechercher dans l’historique de commande (dans terminal)

# Astuces et Bonnes Pratiques

- Ajouter «  | grep mot  »  après une commande pour trouver un mot. Le «  |  » permet de concaténer plusieurs commande (passer le résultat d’une commande à une autre).
- Quand on  ne connait pas la taille du fichier -> cat, grep, less... mais pas nano ou vim. Pour fichier compressé (gzip) -> zcat, zgrep, zless

# Commandes

### Orientation
```bash
history    // affiche historique de commande

ls -l      // liste en mode détaillé (droits etc)
ls -l *.conf | sort    // liste et trie avec détails les fichiers .conf
ls -lah                // liste et trie en plus lisible
ls -a         // liste les fichiers cachés
dump      // info au format hexa

who     // liste des terminaux ouverts

users   // liste des utilisateurs connectés

ps     // liste des processus

ps uww num_PID    // infos sur un processus

top    // info consommation ressources

sudo -H -i    // être comme root mais sur un utilisateur lambda

grep   // permet de filtrer le résultat de la commande

commande >> fichier   // ajouter résultat dans un fichier (écrase pas)
commande > fichier    // ajouter résultat dans un fichier (écrase)

apt purge mon_paquet    // en cas de problèmes de installation
```

### Répertoires
```bash
mkdir nrepertoire                   // créer 1 répertoire

mkdir -p /repertoire1/repertoire2   // plusieurs répertoires en même temps

rm -r repertoire                    // supprimer

mv /home /home.old           // renomme /home en /home.old

mv /home/coucou/ /tmp/       // déplace /coucou dans /tpm
```

### Fichiers
```bash
touch nom_fichier           // créer

rm nom_fichier              // supprimer

rm -r *.txt                 // supprimer tout les fichiers txt

stat nom_fichier            // info sur fichier
```

### Utilisateurs et groupes
```bash
adduser nom_utilisateur  // ajouter utilisateur

addgroup nom_groupe      // ajouter groupe

id nom_utilisateur       // info utilisateur (uid,gid...)

usermod -G nom_groupe nom_user  // ajouter un utilisateur à un groupe

chgrp nom_groupe /repertoire    // changer groupe propriétaire de repertoire

cat /etc/group      // affiche la liste des groupes

cat /etc/passwd     // affiche la liste des utilisateurs

visudo /etc/sudoers    // editer fichier de conf sudo
```

### Accès (ACL)
```bash
chmod g+rwx /repertoire  // donner droits rwx au groupe sur repertoire
chmod u+rwx fichier      // droits pour utilisateur sur fichier
chmod a-rwx              // enlever droits pour tout le monde
chmod 1777 /repertoire   // groupe proprio de repertoire a tout les droits sauf supprimer, seul le créateur peut supprimer

setfacl -m g:nom_groupe:rwx /repertoire  // droits rwx au groupe sur repertoire
setfacl -m u:nom_groupe:rwx /repertoire  // pareil pour utilisateur
setfacl -m g:nom_groupe:0 /repertoire    // enlève tout les droits au groupe sur repertoire

getfacl /repertoire  // liste les acl de repertoire
```

### Stockage
voir [[Mémoire]]
```bash
lsblk      // info disques et partitions

ls -l /dev/disk/by-uuid/    // identifier les UUID des partitions

df -hT   // liste ce qui est monté

fdisk -l         // info disques et partitions
fdisk /dev/sdb   // monter nouvelle partition sur le disque sdb
>m       // help
>n       // ajouter partition
>p       // primaire
>+500    // de 500 Mo
>t       // modifier type
>0c      // FAT32 LBA
>w       // sauvegarder

mkfs.vfat /dev/sdb1   // formate sdb1 en FAT
mkfs.ext4 /dev/sdb1   // formate sdb1 en ext4

mount /dev/sdb1 /repertoire  // monter partition dans repertoire

cat /etc/fstab   // table de montage (option noexec, noauto...)

mount -a -o remount    // remonte tout ce qui est déclaré dans le fstab

mount -o remount chemin    // remonte la partition chemin
```
Pour mettre un dossier existant dans une nouvelle partition :
- Créer un nouveau dossier
- Le monter dans une nouvelle partition
#### Disques virtuels
voir [[Démarrage Système#Disques virtuels|Disques virtuels]]
```bash
pvs   // liste les PV

vgs   // liste les VG

lvs   // liste les LV

pvdisplay /dev/sda2          // plus de détails sur le PV
vgdisplay systemvol          // plus de détails sur le VG
lvdisplay /dev/systemvol/<LV au choix>     // plus de détails sur le LV

growpart /dev/sda 2     // met le reste d’espace disque dans la partition sda2

pvresize /dev/sda2      // adapte la taille de pv à l’espace disponible

lvextend -L 2G -r /dev/systemvol/home      // 2Go pour le LV home
lvextend -L +2G -r /dev/systemvol/home     // augmente le LV de 2Go
```

### Système
```bash
dmesg              // affiche log noyau
dmesg -H           // plus lisible
dmesg | grep eth   // info ethernet

free -h    // taille de la RAM

cat /proc/cpuinfo   // info cpu
cat /etc/systemd/logind.conf   // inittab

/etc/apt/sources.list    // là où OS va chercher les maj
/dev     // les devices
/var     // les logs

systemctl list-timers    // liste les timers
```

### Réseaux
```bash
wget adress_web    // récupère index.html

iptables-save   // sauvegarder iptables et affiche le contenu du Firewall

nano /etc/iptables/rules.v4    // les règles du Firewall (iptables)

tracert adress_ip   // comme ping mais avec plus infos

nslookup www.site.com    // affiche le serveur DNS configuré par défaut (cette commande dispose de nombreuses options permettant de tester et de vérifier le processus DNS de manière approfondie)

lshw -C network   // info carte réseaux

netstat -laputen    // liste toute les connexions active

ss -laputen     // nouveau netstat (plus info)

route -n      // affiche les ip et leur passerelles et autres info

```
