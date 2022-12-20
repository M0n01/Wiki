## Disques virtuels

**LVM** = créé des disques virtuel dans un disque physique (permet d'augmenter la taille des partitions à chaud)

**PV** = Physical Volume (disque/partition). Ne peut contenir que 1 VG.

**VG** = Virtual Group. Contient des LV et peut être sur plusieur PV.

**LV** = Logical Volume.

## Cron, Timer et Démon

**Cron** = Programme pour exécuter automatiquement des scripts, des commandes ou des logiciels à une date et une heure spécifiée précise, ou selon un cycle défini à l’avance.
Chaque utilisateur a un fichier crontab, lui permettant d'indiquer les actions à exécuter.

Par défaut, l'OS utilise les Timers plutôt que les Crons.

**Timer** = Fichier de programmation qui va se charger de lancer des services à intervalles réguliers.

**Service** = Programme qui est exécuté en tâche de fond (sans interaction directe avec l'utilisateur). A noter qu'un service peut être appelé également démon.

Chez systemd, on ne parle pas de service, mais d'unité.