*Installer steghide*

Encrypter un fichier dans une image
```bash
steghide embed -cf chemin_fichier_hébergeur -ef chemin_fichier_à_cacher
```
- Exemple:
```bash
steghide embed -cf /home/image.jpeg -ef /home/texte_secret.txt
```

Ensuite, entrer une passphrase (à retenir)

Décrypter
```bash
steghide extract -sf chemin_fichier_hébergeur
```
- Exemple:
```bash
steghide extract -sf /home/image.jpeg
```