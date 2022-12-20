![[logoWindows.png|500]]

# Commandes


## Bureau à distance

wind + R : 
- SystemPropertiesAdvanced.exe

Utilisation à distance > autoriser > autoriser les connexions > ajouter 1 user

Wind + R : 
- lusrmgr.msc

Utilisateur Bureaux à distance -> ajouter -> User

Wind + R : 
- firewall.cpl | sysdm.cpl

traffic entrant / sortant

## AD

- Utilisateur 

- A
	- G
		- DL
			- P


## GPO

bloquer cmd...

#### Gestion de stratégie de groupe

Créer stratégie dans Objet de stratégie de groupe :
- clique droit sur Objet de stratégie... > nouveau
- clique droit sur GPO > modifier
- Préférences : propose à l'utilisateur
- Parametre Windows > mappage de lecteurs
- clique droit fenêtre > nouveau mappage
- Action : mettre à jour
- Emplacement : chemin dossier partage
- libeller (nom)
- Pas de droits particulier
- Appliquer
	- Commun > ciblage > groupe de secu (option pour condition)
	- (un mappage ciblé l'autre non)
- Clique droit sur OU > lier stratégie

- Paramètres :
	- Ordinateur (s'applique au démarrage de la machine)
	- Utilisateur (s'applique à l'ouverture de session)
- 1 partie arborescence (la même que l'AD) 
- 1 partie avec les gpo (objet de strategie de groupe)
	- appliqué sur les OU (dossiers) de l'arborescence
	- stratégie par défaut
	- stratégie par défaut pour controlleur de domaine (AD)

- **Pour forcer maj de la GPO sur un OU**
	- clique droit sur OU > maj de la stratégie de groupe

- Ciblage :
	- Appliquer GPO que si appartient à tel groupe ou tel OS...

- Savoir déployer une imprimante ou une GPO qu'on veut

Sur post (cmd) :
	gpupdate /force

**Imprimante** :
- intall imprimante en local sur serveur
	- ou Imprimante et scanner > rchercher >Je ne trouve pas
	- imprimante locale
	- adress IP dans le réseau sur la plage ip pour imprimante
	- décocher aller chercher imprimante
	- generic network card
	- on prend n'importe quoi
	- on utilise le driver actuel (pas de remplacement)
	- on partage
	- emplacement -> description (ce qu'on veut)
	- Imprimante et scanner > Liste imprimante clique droit > gérer > propriété imprimante > partage > cocher liste annuair
- install serveur d'impression
- Propriete de l'imprimante > Partage et lister dans l'annuaire
- Nouvelle GPO
- modifier GPO
- Préférences > Param panneau > imprimante
- Nouvelle > mettre a jour > 