# Laravel Docker Boilerplate (TCP)

Минималистичный базовый проект для Laravel на стеке PHP-FPM + Apache(httpd) (через TCP порт 9000) + PostgreSQL + pgAdmin.

## Структура проекта
- `src/` — исходный код Laravel (монтируется в `/var/www/laravel`)
- `config/` — конфигурация Apache и PHP
- `docker/` — Dockerfile для образов
- `.env.docker` — переменные окружения для Docker-контейнеров

## Быстрый старт

1. **Настройка окружения:**
   Скопируйте пример файла окружения:
   ```bash
   cp .env.docker.example .env.docker
   ```

2. **Создание нового проекта Laravel:**
   > **Важно:** Для создания нового проекта папка `src/` должна быть пустой.
   ```bash
   make laravel-install
   ```
   Эта команда поднимет контейнеры, установит свежий Laravel в директорию `src/` и выполнит миграции. База данных будет сразу готова к работе.

3. **Если проект уже существует (копирование своего проекта в src):**
   ```bash
   make up
   make migrate
   ```
   После копирования файлов обязательно запустите миграции.

4. **Доступ к сервисам:**
    - Web App: [http://localhost](http://localhost)
    - pgAdmin: [http://localhost:8080](http://localhost:8080) (email: `admin@example.com`, pass: `admin`)
    - PostgreSQL: `localhost:5432`

## Основные команды (Makefile)

- `make up` — запуск контейнеров (с предварительной проверкой файлов и настройкой .env)
- `make down` — остановка контейнеров
- `make restart` — перезапуск контейнеров
- `make build` — сборка образов
- `make rebuild` — пересборка образов без кэша
- `make xdebug-up` — запуск с включенным Xdebug
- `make xdebug-down` — остановка сервисов Xdebug
- `make logs` — просмотр логов всех контейнеров
- `make status` — статус контейнеров
- `make info` — информация о портах и сервисах
- `make validate` — проверка доступности сервисов по HTTP

### Laravel и Composer
- `make laravel-install` — установка чистого Laravel в `src/` (full cycle)
- `make artisan CMD="migrate"` — выполнение команд artisan
- `make composer CMD="install"` — выполнение команд composer
- `make migrate` — выполнение миграций
- `make rollback` — откат миграций
- `make fresh` — пересоздание БД и запуск сидов
- `make tinker` — запуск Laravel Tinker
- `make test-php` — запуск тестов (PHPUnit)
- `make permissions` — исправление прав на storage/cache
- `make composer-install` — установка зависимостей
- `make composer-update` — обновление зависимостей

### Утилиты и отладка
- `make shell-php` — вход в консоль PHP-контейнера
- `make shell-httpd` — вход в консоль Apache-контейнера
- `make shell-postgres` — вход в консоль PostgreSQL (psql)
- `make logs-php` — логи PHP-FPM
- `make logs-httpd` — логи Apache
- `make clean` — удаление контейнеров и томов (очистка БД)
- `make clean-all` — полная очистка (контейнеры, образы, тома)
- `make dev-reset` — полный сброс и перезапуск среды

## Особенности
- **Production-ready Apache:** Использование `AllowOverride None` для максимальной производительности. Файл `.htaccess` в `src/public` не требуется (и удаляется при установке), так как все правила маршрутизации Laravel прописаны напрямую в `config/httpd/httpd.conf` через `SetHandler` и `mod_rewrite`.
- Связь Apache и PHP через **TCP** (`laravel-php-httpd-tcp:9000`).
- DocumentRoot Apache настроен на `src/public`.
- Весь проект Laravel находится в подпапке `src/`, что позволяет держать конфигурацию Docker отдельно от кода приложения.
- PostgreSQL 17.
- Готовый Dockerfile со всеми необходимыми расширениями для Laravel (pdo_pgsql, gd, intl, zip, opcache, xdebug и др.).