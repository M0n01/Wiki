***voir TP pour procédure***

**Quelles sont les informations supervisées de la Vm ?**
- Voir les infos
	- proxmox1 -> 100(VMdsl) -> Résumé

**Peut on créer un snapshot ou une sauvegarde ?**
- Oui
- proxmox1 -> 100(VMdsl) -> Snapshots

**Est il possible de créer des droits pour une Vm vis à vis d'un utilisateur ou un groupe particulier ?**
- Oui
- proxmox1 -> 100(VMdsl) -> Permissions

**Quel(s) rôle(s) doit on attribuer pour que cette personne puisse seulement utiliser cette Vm sans possibilité d'en changer la configuration ?**
- PVE Admin : can do most things, but miss rights to modify system settings (Sys.PowerMgmt, Sys.Modify, Realm.Allocate).