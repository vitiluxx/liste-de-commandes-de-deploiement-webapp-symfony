## ----------------------------------- ------------------------------------------------------

NOTE IMPORTANTE (debut) : PHILOSOPHIE DE DÉVELOPPEMENT DU PROJET
- Le développement des l'interfaces utilisateurs repose sur une autonomie absolue du front-end. Afin de garantir l'indépendance totale du rendu et d'éviter toute dépendance vis-à-vis de ressources tierces comme Bootstrap, l'application doit fonctionner exclusivement de manière locale. Aucune dépendance ou ressource externe nécessitant une connexion Internet ne doit être intégrée, le design étant intégralement codé en CSS brut.

- Concernant les conventions de code, la logique métier de l'application doit etre majoritairement rédigée en français(noms de class, noms des fonctions, noms des variables, noms des fichiers). Néanmoins, pour prévenir tout conflit lié à l'encodage de caractères UTF-8, le recours aux accents est strictement interdit dans l'ensemble des nommages. Une exception est accordée aux termes techniques critiques et essentiels au fonctionnement des frameworks ou des serveurs, tels que le mot comme index, login, render noms des bibliotheque etc, qui doivent obligatoirement conserver leur nomenclature d'origine en anglais.

- Enfin, l'architecture du projet doit imposer une isolation modulaire stricte. La partie web est totalement autonome et l'intégralité des fichiers nécessaires à son fonctionnement doit demeurer confinée dans le répertoire ./projet/. De la même manière, la partie desktop doit jouir d'une autonomie complète et regroupe tous ses composants exclusifs dans le dossier ./desktop/. Tous les éléments situés à la racine ./ sont quant à eux dédiés uniquement à la configuration globale des ressources utilisées et à la documentation, ceux-ci peuvent eventuellement intervenir ou permettre aux étapes comme la compilation ou de build de l'application.

NOTE IMPORTANTE (fin).

## ----------------------------------- ------------------------------------------------------

Effectue un audit technique approfondi du code source actuel pour identifier tout mécanisme existant de gestion d'exceptions ou de middlewares d'erreurs, puis, si une structure est présente, optimise-la pour en garantir la robustesse et la centralisation selon les standards du framework ; à l'inverse, si aucun système n'est détecté, conçoit une architecture globale de gestion d'erreurs incluant des templates personnalisés pour les codes HTTP majeurs (400, 401, 403, 404, 429, 500, etc), en veillant à ce que chaque réponse soit standardisée avec des métadonnées précises (code d'erreur unique, message personnalisé, timestamp) et que le système permette une capture automatique des exceptions tout en intégrant une journalisation sécurisée des erreurs critiques.

## ----------------------------------- ------------------------------------------------------

Plan d'implémentation du système de réservation de stock
Analyse du problème actuel :
❌ Aucun mécanisme de réservation : MAGASIN peut créer des transferts sans vérifier si le stock est suffisant
❌ Aucun verrouillage : MAGASIN peut continuer à utiliser le stock réservé
❌ Risque de stock négatif : Si MAGASIN utilise le stock réservé, il peut se retrouver en stock négatif
Solution proposée :
1. Ajouter un champ quantite_reservee dans l'Entity Stock

Ajouter quantite_reservee (DECIMAL) pour stocker les quantités réservées par les transferts en attente
Stock disponible réel = quantite - quantite_reservee
2. Modifier la logique de création de transfert (MAGASIN)

Vérifier que quantite - quantite_reservee >= quantite_transfert
Si OK : incrémenter quantite_reservee du stock MAGASIN
Si KO : refuser la création du transfert avec message "Stock insuffisant"
3. Modifier la logique de confirmation (BAR → Serveur)

Côté BAR : Augmenter le stock BAR lors de la confirmation
Côté Serveur :
Diminuer quantite_reservee du stock MAGASIN
Diminuer quantite du stock MAGASIN
Créer événement transfer_confirmed
4. Modifier la logique de rejet (BAR → Serveur)

Côté BAR : Aucun changement de stock
Côté Serveur :
Libérer quantite_reservee du stock MAGASIN (décrémenter)
Mettre mouvement à REJECTED
Créer événement transfer_rejected
5. Ajouter une interface de gestion des réservations

Page "Réservations en attente" pour MAGASIN
Possibilité d'annuler un transfert en attente (libère la réservation)
## ----------------------------------- ------------------------------------------------------
