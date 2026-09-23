# Laravel API — Ambiente Docker de Estudo

Ambiente completo com Nginx + PHP-FPM 8.3 + MySQL 8 + Redis + Adminer + Mailhog.

## Estrutura

```
laravel-api-docker/
├── docker-compose.yml
├── .env                     <- variáveis do docker-compose (senhas do banco etc.)
├── docker/
│   ├── php/Dockerfile
│   ├── nginx/default.conf
│   └── mysql/my.cnf
└── src/                     <- aqui vai ficar o projeto Laravel
```

## Passo a passo

### 1. Subir os containers (ainda sem o Laravel instalado)

```bash
docker compose up -d --build
```

### 2. Criar o projeto Laravel dentro do container

A pasta `src/` está vazia de propósito — vamos instalar o Laravel de dentro do container `app`, pra não depender de PHP/Composer no seu host:

```bash
docker compose run --rm app composer create-project laravel/laravel .
```

> Se der erro de permissão, rode antes: `sudo chown -R $USER:$USER src/`

### 3. Configurar o `.env` do Laravel (dentro de `src/.env`)

Depois que o projeto for criado, edite `src/.env` para apontar pro banco e pro mailhog do Docker (não use `127.0.0.1`, use o nome do serviço):

```env
DB_CONNECTION=mysql
DB_HOST=db
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=laravel
DB_PASSWORD=secret

REDIS_HOST=redis
REDIS_PORT=6379

MAIL_MAILER=smtp
MAIL_HOST=mailhog
MAIL_PORT=1025
MAIL_ENCRYPTION=null
```

### 4. Rodar as migrations

```bash
docker compose exec app php artisan migrate
```

### 5. Acessos

| Serviço          | URL                              |
|-------------------|-----------------------------------|
| API Laravel        | http://localhost:8000             |
| Adminer (DB GUI)   | http://localhost:8080             |
| Mailhog (e-mails)  | http://localhost:8025             |

No Adminer, use:
- **Sistema:** MySQL
- **Servidor:** db
- **Usuário:** laravel
- **Senha:** secret
- **Base de dados:** laravel

### Comandos úteis

```bash
# entrar no container da aplicação
docker compose exec app bash

# rodar artisan
docker compose exec app php artisan tinker
docker compose exec app php artisan make:controller Api/UserController --api

# rodar composer
docker compose exec app composer require laravel/sanctum

# ver logs
docker compose logs -f app

# derrubar tudo
docker compose down

# derrubar tudo e apagar o banco também
docker compose down -v
```

## Dica pra estudar API

Depois do projeto instalado, um bom próximo passo é configurar o **Laravel Sanctum** para autenticação de API:

**Passo 1: Entrar no container**
```bash
docker compose exec app bash
```
**Passo 2: Confirmar que o Composer está lá**
```bash
composer --version
```
Só pra confirmar que está tudo certo antes de instalar.
**Passo 3: Instalar o Laravel**
Ainda dentro do container:
```bash
composer create-project laravel/laravel .
```
O **.** instala na pasta atual (/var/www), que é o src/ do seu host.

**OBS:** caso não consiga instalar o laravel, remova todos os arquivos/pastas do dir xxx/var/www$
```bash
rm -R public/
```

Ou...

```bash
docker compose exec app composer require laravel/sanctum
docker compose exec app php artisan vendor:publish --provider="Laravel\Sanctum\SanctumServiceProvider"
docker compose exec app php artisan migrate
```
