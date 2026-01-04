# Laravel Docker Boilerplate (TCP)

A minimalist base project for Laravel using the PHP-FPM + Apache(httpd) stack (TCP) + PostgreSQL + pgAdmin.

## Project Structure
- `src/` — Laravel source code (mounted to `/var/www/laravel`)
- `config/` — Apache and PHP configuration
- `docker/` — Dockerfiles for images
- `.env.docker` — environment variables for Docker containers

## Quick Start

1. **Environment Setup:**
   Copy the example environment file:
   ```bash
   cp .env.docker.example .env.docker
   ```

2. **Creating a New Laravel Project:**
   > **Important:** To create a new project, the `src/` folder must be empty.
   ```bash
   make laravel-install
   ```
   This command will start the containers, install a fresh Laravel instance into the `src/` directory, and run migrations. The database will be ready to use immediately.

3. **If the project already exists (copying your own project to src):**
   ```bash
   make up
   make migrate
   ```
   Be sure to run migrations after copying your files.

4. **Accessing Services:**
    - Web App: [http://localhost](http://localhost)
    - pgAdmin: [http://localhost:8080](http://localhost:8080) (email: `admin@example.com`, pass: `admin`)
    - PostgreSQL: `localhost:5432`

## Key Commands (Makefile)

- `make up` — start containers (with file check and .env setup)
- `make down` — stop containers
- `make restart` — restart containers
- `make build` — build images
- `make rebuild` — rebuild images without cache
- `make xdebug-up` — start with Xdebug enabled
- `make xdebug-down` — stop Xdebug services
- `make logs` — view logs of all containers
- `make status` — container status
- `make info` — information about ports and services
- `make validate` — check service availability via HTTP

### Laravel and Composer
- `make laravel-install` — install fresh Laravel in `src/` (full cycle)
- `make artisan CMD="migrate"` — run artisan commands
- `make composer CMD="install"` — run composer commands
- `make migrate` — run migrations
- `make rollback` — rollback migrations
- `make fresh` — recreate DB and run seeds
- `make tinker` — run Laravel Tinker
- `make test-php` — run tests (PHPUnit)
- `make permissions` — fix storage/cache permissions
- `make composer-install` — install dependencies
- `make composer-update` — update dependencies

### Utilities and Debugging
- `make shell-php` — enter PHP container shell
- `make shell-httpd` — enter Apache container shell
- `make shell-postgres` — enter PostgreSQL shell (psql)
- `make logs-php` — PHP-FPM logs
- `make logs-httpd` — Apache logs
- `make clean` — remove containers and volumes (clear DB)
- `make clean-all` — full cleanup (containers, images, volumes)
- `make dev-reset` — full reset and restart of the environment

## Features
- **Production-ready Apache:** Uses `AllowOverride None` for maximum performance. The `.htaccess` file in `src/public` is not required (and is removed during installation) because all Laravel routing rules are defined directly in `config/httpd/httpd.conf` using `SetHandler` and `mod_rewrite`.
- Apache and PHP communication via **TCP** (`laravel-php-httpd-tcp:9000`).
- Apache DocumentRoot configured to `src/public`.
- The entire Laravel project is located in the `src/` subfolder, keeping Docker configuration separate from application code.
- PostgreSQL 17.
- Ready-to-use Dockerfile with all necessary extensions for Laravel (pdo_pgsql, gd, intl, zip, opcache, xdebug, etc.).
