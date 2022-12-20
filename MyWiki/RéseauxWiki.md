## Connexion à distance

#### SSH :
Protocole permettant d'établir une communication chiffrée, donc sécurisée (on parle parfois de _tunnel_), sur un réseau informatique (intranet ou Internet) entre une machine locale (le _client_) et une machine distante (le _serveur_). La sécurité du chiffrement peut être assurée par différentes méthodes, entre autre par mot de passe ou par un système de clés publique/privée (mieux sécurisé, on parle alors de [cryptographie asymétrique](https://fr.wikipedia.org/wiki/cryptographie%20asym%C3%A9trique "https://fr.wikipedia.org/wiki/cryptographie asymétrique")).

Pour le RSA prendre RSA 4096 car 2048 trop juste.

Lors d'une connexion ssh à un serveur, l'hôte sauvegarde la clé publique du serveur. Si elle vient à changer (dans le cas d'un Man In The Middle ou d'un changement sur le serveur) alors on sera prévenu de ce changement avant de se connecter par sécurité.

**Bonne pratique** = Désactiver la connexion via SSH à root avec un mot de passe. Donc obligation de se connecter via une clé.


#### VNC
#### RDP
#### NoVNC
#### HTTP (HTTPS)


### Solutions déploiement systèmes

- Wsus   --> Serveur pour maj
- Sysprep --> image custom pour déploiement (faire poste par poste en physique)
- WDS  /  FOG
- MDT  -> très pertinent
- SCCM (Payant)
- AutoPilot
