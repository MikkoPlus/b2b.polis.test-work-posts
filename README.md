# b2b.polis.test-work-posts

Мини‑приложение, в котором можно создавать статьи и оставлять к ним комментарии.  
Фронтенд реализован на React + TypeScript, бэкенд — на Laravel с REST API и MySQL.

---

## Технологии

- **Backend**
  - PHP 8.2+
  - Laravel 12
  - MySQL 8
  - PHPStan + Larastan (статический анализ)
  - PHPUnit (тесты)
- **Frontend**
  - React
  - TypeScript
  - Vite
  - React Router
  - SCSS‑modules
- **Инфраструктура**
  - Docker, Docker Compose
  - Nginx

---

## Структура проекта

- `back/` — Laravel API (модели `Article`, `Comment`, контроллеры, миграции и т.д.)
- `front/` — React‑клиент
- `docker/` — Dockerfile для PHP и конфиг Nginx
- `docker-compose.yml` — оркестрация контейнеров (app, frontend, nginx, db)

---

## Запуск через Docker

### 0. Клонирование репозитория

В проекте используются git submodules, поэтому для корректного клонирования репозитория и установки основной ветки используйте команду

```bash
  git clone --recurse-submodules git@github.com:MikkoPlus/b2b.polis.test-work-posts.git
  cd b2b.polis.test-work-posts
  git submodule foreach 'git checkout main && git pull'
```

### 1. Подготовка

#### 1.1 Подготовка `.env` для Laravel

В папке `back`:

```bash
cp .env.example .env
```

Пример настроек, соответствующих `docker-compose.yml`:

```env
DB_CONNECTION=mysql
DB_HOST=db
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=laravel
DB_PASSWORD=root

CACHE_STORE=file
SESSION_DRIVER=file
QUEUE_CONNECTION=sync
```

Установите пакеты composer

```bash
composer install
```

Сгенерируйте ключ приложения:

```bash
php artisan key:generate
```

#### 1.2 Подготовка `.env` для react

В папке `front`:

```bash
touch .env && echo "VITE_API_URL=http://localhost:8000" > .env
```

### 2. Запуск контейнеров

В корне репозитория:

```bash
docker-compose up --build
```

После запуска:

- API (Laravel) доступен через Nginx по адресу `http://localhost:8000` (эндпоинты `/api/...`).
- Frontend (Vite) — `http://localhost:5173`.

### 3. Миграции

Внутри контейнера приложения или локально:

```bash
cd back
php artisan migrate
```

### 4. Сидер

Внутри контейнера приложения или локально:

```bash
cd back
php artisan bd:seed
```

---

## Локальный запуск без Docker (опционально)

### Backend

```bash
cd back
cp .env.example .env
composer install

php artisan key:generate
php artisan migrate
php artisan serve
```

Пример настроек под локальный MySQL (например, XAMPP):

```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=root
DB_PASSWORD=

CACHE_STORE=file
SESSION_DRIVER=file
QUEUE_CONNECTION=sync
```

### Frontend

```bash
cd front
npm install
touch .env && echo "VITE_API_URL=http://localhost:8000" > .env
npm run dev
```

---

## Рабочее мини приложение доступно по адресу http://45.12.130.231:5173

---

## Проверка кода и тесты

В папке `back`:

```bash
vendor/bin/phpstan analyse
php artisan test
```
