# liste-de-commandes-de-deploiement-webapp-symfony

A) En local (build + préparation)

# dépendances PHP
composer install

### dans .env modifier APP_ENV=dev en APP_ENV=prod

### générer une valeur pour " APP_SECRET "
	php -r "echo bin2hex(random_bytes(32));"

###  installer le package Apache Pack, qui génère automatiquement un fichier .htaccess dans le dossier public/
composer require symfony/apache-pack

### Nettoyer le projet avant envoi
	php bin/console cache:clear --env=prod
	php bin/console cache:warmup --env=prod


B) Sur le serveur (déploiement) VIA SSH 
composer install --no-dev --optimize-autoloader

# migrations des tables
php bin/console doctrine:migrations:migrate --env=prod --no-interaction

# Publier/mapper les assets Symfony vers public/ (important en prod)
php bin/console asset-map:compile --env=prod

# cache
composer dump-autoload --optimize --classmap-authoritative
php bin/console cache:clear --env=prod --no-debug
php bin/console cache:warmup --env=prod

### CREER UN FICHIER htaccess qui redirige vers le rep public/ 
```contenu fichier 

<IfModule mod_rewrite.c>
    RewriteEngine On

    # Si la requête ne concerne pas déjà le dossier public
    RewriteCond %{REQUEST_URI} !^/public/

    # Rediriger toutes les requêtes vers le dossier public
    RewriteRule ^(.*)$ public/$1 [L]
</IfModule>

```
