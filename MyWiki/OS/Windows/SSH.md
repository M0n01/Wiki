
Exemple, une machine windows se connecte en ssh à une machine linux :
- Dans PuttyGen:
	- Générer une clé privé 
	- Charger la clé privé
	- Récupérer la clé publqiue correspondante

Copier la clé publique et la coller dans authorized_keys. Voir  [[Sys linux]]
(Par exemple se connecter en ssh sans clé pour faire le copier coller)
- Dans Putty:
	- Session > rentrer ip de la machine cible
	- SSH > Auth > Browse > prendre la clé privé
	- Open

**Mettre un nom sur une adresse IP** :
Ouvrir en admin
- Disque local C > Windows > System32 > drivers > etc > editer le fichier hosts 
- Bombarder le CTRL+S