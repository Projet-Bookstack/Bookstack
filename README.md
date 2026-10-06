# Bookstack

Plateforme de documentation et de wiki auto-hébergée, basée sur BookStack (PHP / Laravel), organisée en **étagères, livres, chapitres et pages**.

Ce dépôt contient le code de l'application, sa configuration de déploiement (Docker Stack) ainsi que les outils de test et de qualité utilisés dans le cadre du projet.

> Infrastructure associée : Projet-Bookstack/Bookstack-infrastructure(https://github.com/Projet-Bookstack/Bookstack-infrastructure)

## Sommaire

- Fonctionnalités
- Stack technique
- Structure du dépôt
- Prérequis
- Installation en local
- Configuration
- Déploiement
- Tests et qualité
- Tests de charge
- CI/CD
- Contribuer
- Licence

## Fonctionnalités

- Organisation du contenu en étagères, livres, chapitres et pages
- Éditeur WYSIWYG et Markdown
- Gestion des utilisateurs, rôles et permissions
- Recherche plein texte
- Thèmes et traductions
- API REST et CLI d'administration (`bookstack-system-cli`)

## Stack technique

| Couche | Technologies |
|---|---|
| Backend | PHP, Laravel (`composer.json`, `artisan`) |
| Frontend | TypeScript / JavaScript (`package.json`, `tsconfig.json`) |
| Base de données | MySQL / MariaDB |
| Conteneurs | Docker, Docker Stack (`docker-stack.yml`) |
| Tests | PHPUnit, Jest, Locust |
| Qualité | PHPStan, PHP_CodeSniffer, ESLint, SonarQube |
| CI/CD | GitLab CI (`.gitlab-ci.yml`), Forgejo / GitHub (`.forgejo`, `.github`) |

## Structure du dépôt

| Dossier / fichier | Rôle |
|---|---|
| `app/` | Code applicatif (contrôleurs, modèles, services) |
| `bootstrap/` | Initialisation du framework |
| `database/` | Migrations et seeders |
| `dev/` | Outils et configuration de développement |
| `lang/` | Fichiers de traduction |
| `public/` | Point d'entrée web et assets compilés |
| `resources/` | Vues, JS/TS et styles sources |
| `routes/` | Définition des routes |
| `storage/` | Logs, cache, fichiers uploadés |
| `tests/` | Tests PHP |
| `themes/` | Thèmes personnalisés |
| `docker-stack.yml` | Déploiement en stack Docker |
| `locustfile.py` | Scénarios de tests de charge |
| `.env.example` / `.env.example.complete` | Modèles de configuration |

## Prérequis

- PHP (version compatible avec `composer.json`) et Composer
- Node.js et npm
- MySQL ou MariaDB
- Docker (pour le déploiement conteneurisé)

## Installation en local

```bash
# 1. Cloner le dépôt
git clone https://github.com/Projet-Bookstack/Bookstack.git
cd Bookstack

# 2. Installer les dépendances
composer install
npm install

# 3. Configurer l'environnement
cp .env.example .env
php artisan key:generate

# 4. Créer la base et lancer les migrations
php artisan migrate

# 5. Compiler le frontend
npm run build

# 6. Lancer le serveur de développement
php artisan serve
```

L'application est ensuite accessible sur `http://localhost:8000`.

## Configuration

Copiez `.env.example` vers `.env` puis adaptez au minimum :

```dotenv
APP_URL=https://bookstack.example.com
DB_HOST=localhost
DB_DATABASE=bookstack
DB_USERNAME=bookstack
DB_PASSWORD=change-me
```

`.env.example.complete` liste l'ensemble des options disponibles (stockage S3, authentification, mail, etc.).

> Ne commitez jamais votre fichier `.env`.

## Déploiement

Déploiement avec Docker Stack :

```bash
docker stack deploy -c docker-stack.yml bookstack
```

Pour provisionner l'infrastructure (réseau, load balancer, base de données, etc.), voir le dépôt Bookstack-infrastructure(https://github.com/Projet-Bookstack/Bookstack-infrastructure).

## Tests et qualité

```bash
# Tests PHP
php artisan test          # ou : vendor/bin/phpunit

# Tests JavaScript
npm run test              # Jest

# Analyse statique et style
vendor/bin/phpstan analyse
vendor/bin/phpcs
npm run lint              # ESLint
```

L'analyse de qualité est configurée via `sonar-project.properties`.

> Vérifiez les noms exacts des scripts dans `package.json` et `composer.json`.

## Tests de charge

Les scénarios Locust se trouvent dans `locustfile.py` :

```bash
pip install locust
locust -f locustfile.py --host https://bookstack.example.com
```

Interface disponible sur `http://localhost:8089`.

## CI/CD

La pipeline GitLab CI (`.gitlab-ci.yml`) exécute les tests, l'analyse de qualité et le déploiement. Détaillez ici les stages et variables CI utilisés.

## Contribuer

1. Forkez le dépôt et créez une branche : `git checkout -b feature/ma-fonctionnalite`
2. Respectez le style de code (`phpcs`, `eslint`)
3. Ajoutez ou mettez à jour les tests
4. Ouvrez une Pull Request avec une description claire

Merci de lire le code de conduite avant de contribuer.

## Licence

Distribué sous licence **MIT**.

## Remerciements

Projet basé sur [BookStack](https://github.com/BookStackApp/BookStack), créé par Dan Brown et ses contributeurs.
