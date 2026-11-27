# Smart Tenant SaaS (v2.4) - Bhadawala

A multi-tenant Property, Tenant, and Rental Management SaaS built on Laravel 9 and PHP 8.2 with full Docker support.

---

## Prerequisites

Before starting, ensure you have the following installed on your machine:
- **Git**
- **Docker** (version 20.10+ or newer)
- **Docker Compose** (v2 or `docker-compose`)

---

## Quick Setup with Docker

Follow these step-by-step instructions to clone, set up, and run the project using Docker:

### 1. Clone the Repository

```bash
git clone https://github.com/me-engi/bhadawala.git
cd bhadawala
```

*(If you already have the repository cloned, pull the latest changes:)*
```bash
git pull origin main
```

---

### 2. Configure Environment (`.env`)

If `.env` does not exist, copy `.env.example`:
```bash
cp .env.example .env
```

Ensure the database settings in your `.env` match the Docker Compose configuration:
```dotenv
APP_NAME='Smart Tenant SaaS'
APP_ENV=local
APP_KEY=base64:uKQANRMxI6UGKsL/XHzRG0BfgfP+bRjntzHBuPqNtQ4=
APP_DEBUG=true
APP_URL=http://localhost:8080

DB_CONNECTION=mysql
DB_HOST=db
DB_PORT=3306
DB_DATABASE=smart_tenant
DB_USERNAME=smart_tenant
DB_PASSWORD=secret
```

---

### 3. Build & Start the Docker Containers

Build the application image and run the containers in the background:

```bash
docker compose up -d --build
```

Verify that the containers are healthy and running:
```bash
docker compose ps
```
You should see:
- `smart_tenant_app`: running on port `8080` (`http://localhost:8080`)
- `smart_tenant_db`: MySQL 8.0 running internally on port `3306`

---

### 4. Run Migrations & Seed Default Data

Execute database migrations and populate the initial seeders (roles, permissions, admin accounts, and default templates):

```bash
docker compose exec app php artisan migrate --seed --force
```

---

### 5. Mark Installation & Set Permissions

Mark the application as installed so it doesn't redirect to the web installer wizard:

```bash
docker compose exec app bash -c "echo 'installed' > storage/installed"
```

Fix storage permissions and clear cached views:
```bash
docker compose exec app bash -c "chown -R www-data:www-data storage bootstrap/cache && chmod -R 775 storage bootstrap/cache && php artisan cache:clear && php artisan view:clear"
```

---

## Access the Application

Open your browser and navigate to:
👉 **[http://localhost:8080](http://localhost:8080)**

Login page:
👉 **[http://localhost:8080/login](http://localhost:8080/login)**

### Default Credentials

| Role | Email (Username) | Password |
| :--- | :--- | :--- |
| **Super Admin** | `superadmin@gmail.com` | `123456` |
| **Property Owner** | `owner@gmail.com` | `123456` |
| **Tenant** | `tenant@gmail.com` | `123456` |
| **Manager** | `manager@gmail.com` | `123456` |
| **Maintainer** | `maintainer@gmail.com` | `123456` |

---

## Useful Docker Commands

- **Stop containers:**
  ```bash
  docker compose down
  ```

- **Restart containers:**
  ```bash
  docker compose restart
  ```

- **View real-time application logs:**
  ```bash
  docker compose logs -f app
  ```

- **View database logs:**
  ```bash
  docker compose logs -f db
  ```

- **Access application container shell:**
  ```bash
  docker compose exec app bash
  ```

- **Run Artisan commands inside container:**
  ```bash
  docker compose exec app php artisan <command>
  ```
