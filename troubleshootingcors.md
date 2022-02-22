# Pour lister la liste des app nodeJS et les ports
* netstat -lntp | grep node
## Pour les app
* kill numeroPort
## Autoriser un port 
ufw allow 3443

## Pour le problème CORS NodeJS il ne faut surtout pas oublier d'importer cors dans le fichier app.js 
et utiliser app.use(cors())
