# Comandos básicos de Docker e Docker Compose

Ambiente completo com Nginx + PHP-FPM 8.3 + MySQL 8 + Redis + Adminer + Mailhog.

## 🐳 Docker

docker --version — verifica a versão instalada.

docker pull nginx — baixa uma imagem.

docker images — lista as imagens disponíveis.

docker ps — mostra containers em execução.

docker ps -a — mostra todos os containers.

docker run nginx — cria e inicia um container.

docker run -d -p 8080:80 nginx — inicia em segundo plano e mapeia a porta 8080 para 80.

docker stop <container> — para um container.

docker start <container> — inicia novamente um container parado.

docker restart <container> — reinicia um container.

docker rm <container> — remove um container.

docker rmi <imagem> — remove uma imagem.

docker logs <container> — mostra os logs.

docker exec -it <container> bash — abre um terminal dentro do container.

docker inspect <container> — mostra informações detalhadas.


## 🧱 Criando uma imagem

Com um arquivo Dockerfile:
```bash
docker build -t minha-app .
```
Executar:
```bash
docker run -d -p 3000:3000 minha-app
```

## 🐙 Docker Compose
Atualmente, o comando recomendado é docker compose (com espaço), em vez do antigo docker-compose.

Dentro da pasta que contém compose.yaml ou docker-compose.yml:
```bash
docker compose up
```
Iniciar em segundo plano:
```bash
docker compose up -d
```
Parar e remover os containers:
```bash
docker compose down
```
Ver containers:
```bash
docker compose ps
```
Ver logs:
```bash
docker compose logs
```
Acompanhar logs:
```bash
docker compose logs -f
```
Reconstruir as imagens:
```bash
docker compose build
```
Reconstruir e iniciar:
```bash
docker compose up -d --build
```
Reiniciar:
```bash
docker compose restart
```
Executar um comando dentro de um serviço:
```bash
docker compose exec app bash
```
Uma forma simples de memorizar é:
```bash
docker compose exec [SERVIÇO] [COMANDO]
                       │          │
                       │          └── o que executar
                       └───────────── onde executar
```
Exemplo:
```bash
docker compose exec app bash
docker compose exec app ls
docker compose exec app npm install
docker compose exec db psql
```
A ideia é sempre: **"execute X dentro do serviço Y."**

## 📄 Exemplo de compose.yaml
```bash
services:
  app:
    build: .
    ports:
      - "3000:3000"

  db:
    image: postgres:16
    environment:
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: senha
      POSTGRES_DB: minha_db
    ports:
      - "5432:5432"
```
Depois:
```bash
docker compose up -d
```
Isso cria dois serviços: app e db, permitindo que eles funcionem juntos.

