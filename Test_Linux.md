#### Cloud init

ouvrir l'iso
contient : user-data et meta-data

#### Partitionnement
```bash
df -h
lsblk
sudo growpart /dev/sda 2
puis
sudo pvresize /dev/sda2
lvextend -L 2G -r /dev/systemvol/var
lvextend -L 0.5G -r /dev/systemvol/home
lvextend -L 0.125G -r /dev/systemvol/tmp
lvextend -L 8G -r /dev/systemvol/root
```
### details
Déplacé /home dans nouvelle partition :
- renommé home
```bash
mv /home /home.old
```
- créer nouveau répertoire home
- créer une nouvelle partition de 1 Go
```bash
fdisk /dev/sdb   
>m       // help
>n       // ajouter partition
>p       // primaire
>+1G     // de 1G
>w       // sauvegarder
```
- la formater au bon file system
```bash
mkfs.ext4 /dev/sda3
```
- la monter dans le nouveau répertoire home
```bash
mount /dev/sda3 /home
```
- copier le contenu de home.old dans le nouveau home
```bash
cp -rp /home.old/* /home
```
**Monter la partition au démarrage de la machine pour éviter de refaire les manipulations**
- identifier l'UUID de la partition
```bash
ls -l /dev/disk/by-uuid/
``` 
- Modifier le fstab (monte les partitions)
- faire une copie par précaution
```bash
sudo cp /etc/fstab /etc/fstab.old
```
- le modifier
```bash
nano /etc/fstab
```
- ajouter la ligne à la fin
```bash
UUID=Mon_UUID /home ext4 default 0 2
```

### SSH

Pour faire un user privilégié autre que root:
- le mettre dans le groupe sudo
```bash
apt install sudo
usermod -G sudo nom_user
```
Ckeck SSH
```bash
systemctl status ssh.service
```
- Créer une paire de clé rsa 4096 (ATTENTION : TOUS EN localadmin)
```bash
ssh-keygen -t rsa -b 4096
ls -lh .ssh/     '// endroit où sont stockés les clés'
touch .ssh/authorized_keys   '// pour mettre les clés publiques'
cat .ssh/id_rsa.pub >> .ssh/authorized_keys   '// rangement'
chmod 700 .ssh/
chmod 600 .ssh/authorized_keys
chmod 600 .ssh/id_rsa.pub
chmod 600 .ssh/id_rsa
```
Se connecter en ssh avec le mdp et ajouter sa clé publique windows dans authorized_keys.
![image](https://user-images.githubusercontent.com/73076854/207263074-9c3910d3-5bc9-467e-ab8d-c1f43158619b.png)

- dans /etc/ssh/sshd_config (Décommenter, voir ajouter si inexistant)
```bash
Port 22
HostKey /etc/ssh/ssh_host_rsa_key
PermitRootLogin no         '// désactiver accès direct en root'
PubkeyAuthentication yes
AuthorizedKeysFile ....
PasswordAuthentication no  '// désactiver authentification par mdp'
ChallengResponseAuthentication no
```
- Restart
```bash
systemctl restart sshd
```


### PHP
```bash
apt install php7.4-fpm php7.4-mysql php7.4-xml php7.4-mbstring '// Installation des paquets PHP + FPM'
systemctl status php7.4-fpm.service '// Check du service PHP'
sudo mkdir -p /var/www/wordpress/public '// Création du dossier Wordpress/Public/'
sudo useradd -s /usr/sbin/nologin -d /var/www/wordpress -M wpuser '// Création de wpuser sans MDP pour le dossier /var/www/wordpress'
sudo chown -R wpuser:wpuser /var/www/wordpress '// Ajout des droits a wpuser sur /var/www/wordpress'
rm /etc/php/7.4/fpm/pool.d/www.conf '// Suppression du pool FPM par défaut'
nano /etc/php/7.4/fpm/pool.d/wordpress.conf '// Création du pool pour Wordpress'
```
- Fichier de configuration wordpress
```bash
[wordpress] 
chdir = /var/www/wordpress/public 
user = wpuser 
group = wpuser 
listen = /run/php/php7.4-$pool.sock 
listen.mode = 0660 
listen.owner = www-data 
listen.group = www-data 
pm = ondemand 
pm.max_children = 2 
```
- Rédemarrer le service PHP
```bash
systemctl restart php7.4-fpm.service '// Redémarrage de PHP pour application des changements'
```
### Apache2
```bash
apt install apache2 '// Installation du serveur'
apt install iptables '// Installation du firewall'
iptables -I INPUT -p tcp -m tcp --dport 80 -j ACCEPT '// Autorisation du port tcp/80 en INPUT '
iptables -I INPUT -p tcp -m tcp --dport 443 -j ACCEPT '// Autorisation du port tcp/443 en INPUT' 
iptables-save | tee /etc/iptables/rules.v4 '// Sauvegarde de la config IPTABLES'
systemctl reload apache2 '// Reload du service'
a2enmod proxy proxy_fcgi '// Activation des modules'
systemctl restart apache2 '// Restart du service'
nano /etc/apache2/sites-available/wordpress.conf '// Création du VirtualHost pour Wordpress'
```
- Fichier de configuration Apache2 wordpress
```bash
<VirtualHost *:80>
ServerName wordpress.lan
DocumentRoot /var/www/wordpress/public
DirectoryIndex index.php
ProxyPassMatch "^/.*\.php$" "unix:/run/php/php7.4-wordpress.sock|fcgi://localhost/var/www/wordpress/public/"
ErrorLog ${APACHE_LOG_DIR}/wordpress-error.log
CustomLog ${APACHE_LOG_DIR}/wordpress-access.log combined
</VirtualHost>
```
- Activer, puis vérifier
```bash
a2ensite wordpress.conf '// Activer le VirtualHost'
systemctl restart apache2.service '// Relance du service'
apachectl configtest '// Test de configuration'
```
- Ajout d'un override DNS dans le fichier hosts sur l'hôte (C:\Windows\System32\drivers\etc\hosts pour Windows, /etc/hosts pour Linux)
```bash
192.168.XX.XX wordpress.lan
```

### MariaDB
```bash
apt install mariadb-server '// Installation du serveur SQL'
mysql -u root -p '// Connexion console MySQL'
```
- Créer la BDD wpdb, ainsi qu'un utilisateur
```bash
CREATE DATABASE wpdb;
CREATE USER 'wordpressadmin'@'localhost' IDENTIFIED BY 'Toptop=22';
GRANT ALL PRIVILEGES ON wpdb.* TO wordpressadmin@localhost;
FLUSH PRIVILEGES;
exit
```
### FTP
```bash
apt install vsftpd libpam-pwdfile '// Installation du serveur FTP'
iptables -I INPUT -p tcp -m tcp --dport 21 -j ACCEPT '// Autorisation du port tcp/80 en INPUT'
iptables -A INPUT -p tcp --match multiport --dports 20000:20200 -j ACCEPT '// Autorisation des ports tcp/20000 à 20200 en INPUT'
iptables-save | tee /etc/iptables/rules.v4 '// Sauvegarde de la config IPTABLES'
mkdir -p /etc/vsftpd/users.d '// Création dossier de config'
nano /etc/pam.d/vsftpd '// Modifier la configuration PAM pour vsFTPd'
```
- Supprimer toutes les lignes du fichier, puis les remplacer
```bash
auth required pam_pwdfile.so pwdfile /etc/vsftpd/vsftpd.passwd
account required pam_permit.so
session required pam_permit.so
```

```bash
nano /etc/vsftpd.conf '// Fichier de conf'
```
- Ajouter les lignes suivantes en bas du fichier
```bash
pasv_min_port=20000
pasv_max_port=20200
chroot_local_user=YES
allow_writeable_chroot=YES
write_enable=YES
virtual_use_local_privs=YES
guest_enable=YES
hide_ids=YES
user_config_dir=/etc/vsftpd/users.d
```
- Redémarrer le servive vsFTPd
```bash
systemctl restart vsftpd.service '// Redémarrer le démon'
```
- Ajouter les lignes suivantes en bas du fichier
```bash
nano /etc/vsftpd/users.d/wpftp
```
```bash
guest_username=wpuser
local_root=/var/www/wordpress
```
- Créer un mot de passe pour le compte FTP
```bash
echo "wpftp:$(openssl passwd -1)" | tee -a /etc/vsftpd/vsftpd.passwd
```

### WordPress
- Récupérer le dossier Wordpress sur la partage
- Se connecter avec l'utilisateur wpftp sur WinSCP/Filezilla
- Déplacer le dossier Wordpress dans le dossier Public/
```bash
apt install zip '// Installation du paquet'
cd /var/www/wordpress/public '// Changement de répertoire'
unzip wordpress-6.1.1-fr_FR.zip  '// Décompression du fichier'
rm wordpress-6.1.1-fr_FR.zip
cp -rf wordpress/* /var/www/wordpress/public '// Déplacement de tous les fichiers'
rm -rf wordpress/
```
- Finir l'installation sur l'interface WEB de Wordpress
```bash
http://wordpress.lan
```
- Indiquer la BDD, Utilisateur SQL

![image](https://i.imgur.com/9D0RjRm.png)

- Cliquer sur Run the installation

![image](https://user-images.githubusercontent.com/73076854/206922743-8f158e68-325e-4cf5-8d64-4e57e8810708.png)

- Définir le titre, user, password, email

![image](https://user-images.githubusercontent.com/73076854/206922785-f2bd8c90-47f9-47bf-88f2-dda17261cde6.png)

- Cliquer sur Log In

![image](https://user-images.githubusercontent.com/73076854/206922805-9ffc6c7d-467d-450e-92ab-77343ff8c9fe.png)

- Remplir le champ de connexion 

![image](https://user-images.githubusercontent.com/73076854/206922818-1d8d5747-02e0-4e1b-a200-7086eaeacbcd.png)

- Et Voila !
![image](https://user-images.githubusercontent.com/73076854/206922835-a67b5c22-ba82-4033-967f-59b8157ec02a.png)

