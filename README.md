# Trabalhando com Docker

## Introdução

O Docker é uma plataforma de código aberto que simplifica a criação, implantação e execução de aplicativos em contêineres. Este documento serve como uma referência rápida para trabalhar com Docker, incluindo a criação de contêineres, utilização de Dockerfile, Docker Compose e publicação de imagens no Docker Hub.

---

## 1. Criando um Contêiner

Para criar um contêiner utilizando uma imagem específica, utilize o seguinte comando:

```
docker run --name meu-mysql -e MYSQL_ROOT_PASSWORD=minhasenha -d mysql:latest

```
## 2. Dockerfile
Um Dockerfile é um arquivo de texto que contém instruções para construir uma imagem Docker. Abaixo está um exemplo de Dockerfile para criar um contêiner com Java 17 e MongoDB:

```
### Use a imagem oficial do OpenJDK 17
FROM openjdk:17

### Instale o MongoDB
RUN apt-get update && apt-get install -y mongodb

### Defina o diretório de trabalho dentro do contêiner
WORKDIR /app

### Copie o arquivo JAR do seu aplicativo para o contêiner
COPY ./seu-aplicativo.jar /app

### Exponha a porta utilizada pelo MongoDB
EXPOSE 27017

### Exponha a porta do seu aplicativo
EXPOSE 8080

### Comando para iniciar o MongoDB e executar o seu aplicativo quando o contêiner for iniciado
CMD mongod & java -jar seu-aplicativo.jar
```

## 3. Docker Compose

O Docker Compose é uma ferramenta para definir e gerenciar aplicativos Docker multi-contêineres. Abaixo está um exemplo de um arquivo docker-compose.yaml para gerenciar um aplicativo composto por um contêiner Java e outro contêiner MongoDB:

```
version: '3'

services:
  myapp:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "8080:8080"
    depends_on:
      - mongodb
    environment:
      MONGO_URI: mongodb://mongodb:27017
    restart: always

  mongodb:
    image: mongo:latest
    ports:
      - "27017:27017"
    volumes:
      - mongodb_data:/data/db
    restart: always

volumes:
  mongodb_data:
```
### 4. Publicando uma Imagem no Docker Hub
Para publicar uma imagem Docker no Docker Hub, siga os passos abaixo:
#### Faça login no Docker Hub:
```
docker login
```
#### Tag da imagem:
```
docker tag sgmea:latest seu_usuario/docker-sgmea:latest
```
#### Push para o Docker Hub:
```
docker push seu_usuario/docker-sgmea:latest
```
### Atualizando o container por tags:

Depois de construir a aplicação no Docker, atualize a tag da seguinte maneira:

##### Atualizando o container por tags:

```
docker build -t seu_usuario/nome_da_imagem:nova_tag .
```
Exemplo real:

```
docker build -t cedrobuild/sgmea-app:v2.0 .
```
