voir [[RéseauxWiki#Connexion à distance#SSH :|SSH]]

**Sur RaspberryPi:**
Autoriser les connexions SSH
```bash
sudo raspi-config
```
- interfaces options
- enable ssh

**Sur les autres machines:**
Connexion SSH normal
```bash
ssh utilisateur@addresse_ip
```
- Exemple:
```bash
ssh pi@addresse_ip_raspberry
```
Connexion SSH permettant d'ouvrir des interfaces graphiques
```bash
ssh -X pi@addresse_ip_raspberry
```
- Entrer le mot de passe de la RPi

Faire un audit ssh de la machine (à revoir)
- apt install ssh-audit
```bash
ssh-audit adresse_IP
```
