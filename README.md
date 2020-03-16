# Administrer un Serveur VPS
  Ici j'ai fait un condansé de ce qu'il faut pour administrer un serveur VPS,
  c'est à dire comment:
* lier un nom de domaine à son serveur VPS
* créer un sous domaine dans son serveur VPS
* activer le protocle HTTPS
* faire la redirection avec les SPA(Single Page Application) (Angular, React, VueJS)

## Comment configurer un domaine ou un sous domaine
    La configuration du nom de domaine ou sous domaine 
    se passe en deux etapes:
## Configuration du nom de domaine au niveau du du serveur 
## qui heberbe son nom de domaine
1.  Avant tout, il faut aller dans son serveur où se trouve ton nom de domaine
2.  cliquer sur son nom de domaine puis aller la section **Zone DNS**
3.  selon ce qu'on souhaite lier un nom de domaine ou sous domaine
*  sous doamine 
1.   cliquer sur le boutton **Ajouter une entrée** pour ajouter un **sous domaine**
2.   selectionner le sous domaine et ensuite 
3.   cliquer sur le boutton **Modifier l'entrée** puis 
4.  Au niveau du 
*      champ **sous domaine** donnée une valeur ex: **angular**
*      champ **Cible** mêttre l'adresse **IP de son serveur VPS**
*  doamine 
1.   selectionner le  domaine et ensuite 
2.   cliquer sur le boutton **Modifier l'entrée** puis 
3.  Au niveau du 
*      champ **sous domaine** laisser le vide
*      champ **Cible** mêttre l'adresse **IP de son serveur VPS**

## Configuration au niveau de son serveur VPS
1. il faut d'abord set logger sur son VPS avec son ssh
2. Aller dans le repertoire <code>cd /var/www/</code> et créer un dossier 
  du même que le domaine ou sous domaine.<br> 
   Par exemple pour le sous domaine <code>angular.camaratek.com</code>, 
   je crée un dossier avec la commande <code>mkdir angular.camaratek.com</code>
   c'est ce dossier qui va heberger tout les fichiers consernant ce sous domaine
3. et puis après, je me rends au repertoire <code>cd /etc/apache2/sites-available</code>, je créer un fichier de configuration du même nom que 
  le domaine ou sous domaine sauf que j'ajout à la fin **.conf**.<br> 
   Par exemple: <code>touch angular.camaratek.com.conf<code>
   et j'ajoute ces lignes ci-dessous:
<code>
<pre>
&lt;VirtualHost *:80&gt;
    ServerName camaratek.com 
    ServerAlias angular.camaratek.com
    ServerAdmin contact@camaratek.com
    DocumentRoot "/var/www/angular.camaratek.com"
    &lt;Directory /var/www/angular.camaratek.com&gt;
        Options Indexes FollowSymLinks MultiViews
        AllowOverride All
        Order allow,deny
        allow from all
        &lt;/Directory&gt;
    RewriteEngine on
        RewriteCond %{HTTP_HOST} ^camaratek\.com
        RewriteRule ^(.*) /www/$1 [L]
        RewriteCond %{HTTP_HOST} ^([^\.]+)\.camaratek\.com
        RewriteCond /var/www/angular.camaratek.com/%1 -d
        RewriteRule ^(.*) /$1 [L]
&lt;/VirtualHost&gt;
  </pre>
</code>
4. Après j'execute les commande suivantes: 
*  <code>systemctl reload apache2</code>
*  <code>a2ensite camaratek.com.conf</code> 

## pour activer le nom de domain(ou sous domaine) dans le VPS
1. Ensuite j'execute cette commande: "certbot --apache -d angular.camaratek.com"
*   Donner votre Email
*   Answer YES (2) pour que j'obtienne la redirection redirection HTTPS
6. pour la redirection, je créer un fichier .htaccess et je le met à la racine 
   de mon site c'est à dire avec les SPA, quand on tape:
    <code>angualar/home</code> sur la barre de recherche,
    ça ne marche pas sans ce fichier .htaccess avec ce contenu ci-dessous:
<code>
<pre>
    &lt;ifModule mod_rewrite.c&gt;
        RewriteEngine On
        RewriteBase /angular.camaratek.com/
        RewriteCond %{REQUEST_FILENAME} !-f
        RewriteCond %{REQUEST_FILENAME} !-d
        RewriteRule (.*) /index.html [QSA,L]
    &lt;/ifModule&gt;
</pre></code>




 




