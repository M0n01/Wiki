
## TP partie 4

#### page 3

Possible que si on est présent physiquement (pas de ssh).

Connection refused : Port pas en ecoute ou Reject dans iptable

```bash
ss -taupen     // voir les ports en ecoute
ss -tlpen    

systemctl status ssh.service    // voir ssh

systemctl enable ssh.service    // enable

systemctl start ssh.service   // démarrer

nano /etc/ssh/sshd_config     // fichier de conf de ssh (pour modifier le port par exemple ou activer authentification par mdp)

systemctl restart ssh    // redémarrer ssh
systemctl restart ssh.service 

ls -lh .ssh/authorized_keys   // voir les droits sur le fichier ssh (important)

echo 'cle_publique' > .ssh/authorized_keys   // ajouter clé publique dans les clé autorisées

chown

cat /var/log/tomcat9/catalina.out      // fichier important dans tomcat

journalctl -xe      // affiche la fin et avec des explications

```

## Exam

- Apache
- MySQL
- Tomcat
- Xwiki
- PHPmyadmin
- Wordpress