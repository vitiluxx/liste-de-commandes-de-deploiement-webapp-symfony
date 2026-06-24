## ----------------------------------- ------------------------------------------------------

NOTE IMPORTANTE (debut) : PHILOSOPHIE DE DÉVELOPPEMENT DU PROJET
- Le développement des l'interfaces utilisateurs repose sur une autonomie absolue du front-end. Afin de garantir l'indépendance totale du rendu et d'éviter toute dépendance vis-à-vis de ressources tierces comme Bootstrap, l'application doit fonctionner exclusivement de manière locale. Aucune dépendance ou ressource externe nécessitant une connexion Internet ne doit être intégrée, le design étant intégralement codé en CSS brut.

- Concernant les conventions de code, la logique métier de l'application doit etre majoritairement rédigée en français(noms de class, noms des fonctions, noms des variables, noms des fichiers). Néanmoins, pour prévenir tout conflit lié à l'encodage de caractères UTF-8, le recours aux accents est strictement interdit dans l'ensemble des nommages. Une exception est accordée aux termes techniques critiques et essentiels au fonctionnement des frameworks ou des serveurs, tels que le mot comme index, login, render noms des bibliotheque etc, qui doivent obligatoirement conserver leur nomenclature d'origine en anglais.

- Enfin, l'architecture du projet doit imposer une isolation modulaire stricte. La partie web est totalement autonome et l'intégralité des fichiers nécessaires à son fonctionnement doit demeurer confinée dans le répertoire ./projet/. De la même manière, la partie desktop doit jouir d'une autonomie complète et regroupe tous ses composants exclusifs dans le dossier ./desktop/. Tous les éléments situés à la racine ./ sont quant à eux dédiés uniquement à la configuration globale des ressources utilisées et à la documentation, ceux-ci peuvent eventuellement intervenir ou permettre aux étapes comme la compilation ou de build de l'application.

NOTE IMPORTANTE (fin).

## ----------------------------------- ------------------------------------------------------

Effectue un audit technique approfondi du code source actuel pour identifier tout mécanisme existant de gestion d'exceptions ou de middlewares d'erreurs, puis, si une structure est présente, optimise-la pour en garantir la robustesse et la centralisation selon les standards du framework ; à l'inverse, si aucun système n'est détecté, conçoit une architecture globale de gestion d'erreurs incluant des templates personnalisés pour les codes HTTP majeurs (400, 401, 403, 404, 429, 500, etc), en veillant à ce que chaque réponse soit standardisée avec des métadonnées précises (code d'erreur unique, message personnalisé, timestamp) et que le système permette une capture automatique des exceptions tout en intégrant une journalisation sécurisée des erreurs critiques.
