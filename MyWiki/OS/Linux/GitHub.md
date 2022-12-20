![[logoGitHub.png|300]]

*Il faut d'abord créer un repos sur git.*

Copie le repos git sur la machine et lie les deux
```bash
git clone https://adress_repos
```
Ajoute tout le contenu du dossier
```bash
git add .
```
Ajoute juste un fichier
```bash
git add nom_fichier
```
Sauvegarde
```bash
git commit -m "commentaire" 
```
Envoie au repos (la première fois)
```bash
git push -u origin master
```
Envoie au repos
```bash
git push
```
Met à jour la copie locale
```bash
git pull
```


git init : création d'un nouveau dépôt 
git status : affiche l'état du dépôt 
git add : ajout d'un ou plusieurs fichiers dans la zone staging 
git commit : création du commit (SHA-1), intégration à l'historique 
git log : affichage de l'historique des commits 
les 7 premiers caractères du commit
git diff : faire la diff entre des commit
git branch : gestion des branches 
git checkout : déplacement sur une référence (annule les modifs) 
git remote : gestion des dépôts distants 
git pull : récupération des commits depuis un dépôt distant 
git push : envoi de commits vers un dépôt distant
git blame : 


FAIRE des branches pour chaque user

FAIRE authentification ssh par clé pour github
comme ça pas besoin de retapper user et mdp