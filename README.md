# liste-de-commandes-de-deploiement-webapp-symfony

# A) En local (build + préparation)
### creer un fichier .env.local et y inserer les variable(avec leur valeurs) suivants : APP_ENV=prod ; APP_DEBUG=0 ; APP_SECRET=votre_cle_secret_a_generer ; DATABASE_URL=_votre_url

### laisser le fichier ".env" avec son contenu initial a la creation du projet mais exclure(supprimer) les variables qui sont deja presentes dans le fichier ".env.local"

### supprimer la liste des fichier & reertoir suivant : compose.yaml ; compose.override.yaml ; .env.dev ; .env.test ; .env.desktop ; .env.desktop.local ; phpunit.dist.xmll ; tests/ ; var/ ;

### Dépendances PHP
```bash 

composer install

```

### Générer une valeur pour " APP_SECRET "
```bash 

php -r "echo bin2hex(random_bytes(32));"

```
 
###  Installer le package Apache Pack, qui génère automatiquement un fichier .htaccess dans le dossier public/
```bash 

composer require symfony/apache-pack

``` 

### Nettoyer le projet avant envoi
```bash 

	php bin/console cache:clear --env=prod
	php bin/console cache:warmup --env=prod

``` 

# B) Sur le serveur (déploiement) VIA SSH

```bash 

composer install --no-dev --optimize-autoloader

```
 
### Migrations des tables
```bash 

php bin/console doctrine:migrations:migrate --env=prod --no-interaction

```
 
### Publier/mapper les assets Symfony vers public/ (important en prod)
```bash 

php bin/console asset-map:compile --env=prod

``` 

### Gerer le cache
```bash
 
composer dump-autoload --optimize --classmap-authoritative
php bin/console cache:clear --env=prod --no-debug
php bin/console cache:warmup --env=prod

``` 

### LA DERNIERE CHOSE A FAIRE EST DE CREER UN FICHIER ".htaccess" A LA RACINE DU PROJET POUR PERMETTRE DE REDIRIGER VERS LE REPERTOIRE "public/" AINSI LE SERVEUR POURRA LIRE LE FICHIER "index.php"
```contenu fichier 

<IfModule mod_rewrite.c>
    RewriteEngine On

    # Si la requête ne concerne pas déjà le dossier public
    RewriteCond %{REQUEST_URI} !^/public/

    # Rediriger toutes les requêtes vers le dossier public
    RewriteRule ^(.*)$ public/$1 [L]
</IfModule>

```
