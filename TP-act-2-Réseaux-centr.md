
## Config du switch

- vlan 2 : port 8 : communication sur le réseau LanRT
- vlan 10 : port 2 : 
- vlan 11 : port 3 :

Client envoi trame wifi au LAP
LAP retire en tete de couche 1, il reste donc en tete couche 2
Il réencapsule dans CAPWAP
WLC dégage tout pour récup trame avec en tete de couche 2

LAP gère couche 1
WLC gère couche 2

coucou