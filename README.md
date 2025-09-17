# 📘 Guia de Bancos de Dados para Estudos com Docker
## 🔧 1. Pré-requisitos

Antes de começar, certifique-se de ter instalado:

- Docker

- Docker Compose

- Um cliente gráfico para gerenciar os bancos (opcional, mas recomendado):

  - DBeaver ou pgAdmin para PostgreSQL e MySQL
  - RedisInsight para Redis 
  - MongoDB Compass para MongoDB

## 🐳 2. Exemplos docker run (sem Compose)

Caso queira subir os serviços isolados (sem docker-compose), seguem exemplos básicos:
### PostgreSQL
```shell

docker run -d --name postgres_17 \
  --restart always \
  -e POSTGRES_DB=meu_db \
  -e POSTGRES_USER=meu_usuario \
  -e POSTGRES_PASSWORD=minha_senha \
  -p 5432:5432 \
  -v pgData:/var/lib/postgresql/data \
  postgres:17.0
```

### Redis
```shell
docker run -d --name redis_7 \
  --restart always \
  -p 6379:6379 \
  -v rdData:/data \
  redis:7.0 redis-server --appendonly yes
```


### MySQL
```shell
docker run -d --name mysql_8 \
  --restart always \
  -e MYSQL_ROOT_PASSWORD=senha_root_mysql \
  -e MYSQL_DATABASE=meu_db \
  -e MYSQL_USER=meu_usuario \
  -e MYSQL_PASSWORD=minha_senha \
  -p 3306:3306 \
  -v myData:/var/lib/mysql \
  mysql:8.4.6
```


### MongoDB
```shell
docker run -d --name mongo_7 \
  --restart always \
  -e MONGO_INITDB_ROOT_USERNAME=meu_usuario \
  -e MONGO_INITDB_ROOT_PASSWORD=minha_senha \
  -p 27017:27017 \
  -v mongoData:/data/db \
  mongo:7.0
```


### Mongo Express (UI para MongoDB)
```shell
docker run -d --name mongo_express \
  --restart always \
  -e ME_CONFIG_MONGODB_ADMINUSERNAME=meu_usuario \
  -e ME_CONFIG_MONGODB_ADMINPASSWORD=minha_senha \
  -e ME_CONFIG_MONGODB_SERVER=mongo_7 \
  -p 8081:8081 \
  --link mongo_7:mongo \
  mongo-express:1.0.2
```


## 📦 3. Uso com docker-compose

Este repositório já possui um docker-compose.yaml configurado com PostgreSQL, Redis, MySQL e MongoDB + Mongo Express.

### Subindo os serviços
```shell 
docker compose up -d
```

### Parando os serviços
```shell 
docker compose down
```

### Parando os serviços e excluindo containers e volumes
```shell 
docker compose down -v
```

### Removendo todas as imagens
```shell
docker image rm $(docker image ls -aq)
```

### Subindo e recriando (útil após editar variáveis)
```shell
docker compose up -d --force-recreate
```

## 🔌 4. Conexão aos Bancos
Dentro do Docker (entre containers)

- PostgreSQL → postgres:5432
- Redis → redis:6379
- MySQL → mysql:3306
- MongoDB → mongo:27017

Fora do Docker (localhost)

- PostgreSQL → localhost:5432
- Redis → localhost:6379
- MySQL → localhost:3306
- MongoDB → localhost:27017
- Mongo Express → http://localhost:8081

## 🛠️ 5. Comandos Úteis
### Logs
```shell
docker logs -f postgres_17
docker logs -f redis_7
docker logs -f mysql_8
docker logs -f mongo_7
```
### Entrar no container
```shell
docker exec -it postgres_17 psql -U meu_usuario -d meu_db
docker exec -it mysql_8 mysql -u meu_usuario -p
docker exec -it redis_7 redis-cli
docker exec -it mongo_7 mongosh -u meu_usuario -p
```
### Volumes
```shell
docker volume ls
docker volume inspect pgData
```