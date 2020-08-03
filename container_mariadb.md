# Caontainer Docker MariaDB

Uma forma de garantir que sua aplicação irá funcionar  em qualquer ambiente é utilizar Containers. Nesse ambiente estamos utilizando parcialmente esta tecnologia. Por isso nosso PHP e composer rodam localmente mas nosso banco de dados rodará em um container Docker.  

### Instalando o Docker

```bash
sudo apt-get update
```

```bash
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```

Adicionar o repositorio GPG oficinal docker.

```bash
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

Adicionar  o repositorio de intalação.

```bash
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

Install

```bash
sudo apt-get update
```

```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker.io docker-compose
```

Edite o arquivo group e adicione seu usuario ao grupo docker. 

```bash
sudo vim /etc/group
```

```shell
docker:x:998:seunomedeusuario
```

Teste seu docker. 

```bash
docker run hello-world
```

A saída deve ser algo como:

```shell
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
0e03bdcc26d7: Pull complete 
Digest: sha256:49a1c8800c94df04e9658809b006fd8a686cab8028d33cfba2cc049724254202
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

### Criando seu container MariaDB

Crie um arquivo Dockerfile com o conteúdo:

```dockerfile
FROM mariadb:latest
```

E agora um arquivo docker-compose.yml

```docker
version: '3'
services:
    mariadb:
        build:
            context: .
            dockerfile: Dockerfile
        container_name: dbserver
        environment:
            - TZ=America/Sao_Paulo
            - ALLOW_EMPTY_PASSWORD=no
            - MYSQL_ROOT_PASSWORD=lara_root
            - MYSQL_DATABASE=laravel
        ports:
            - 3306:3306   
        volumes:
            - ./database/dbdata:/var/lib/mysql
volumes:
    dbdata:
        driver: local
networks:
  app-tier:
    driver: bridge 
```

Lembrando que estamos dentro do diretorio testeblog da instrução anterior, então crie um diretorio dbdata dentro de database que servirá como deposito dos seus arquivos de banco de dados quando o conteinar nao estiver em uso. 

```bash
mkdir database/dbdata
```

Suba seu container

```bash
docker-compose up --build -d
```

Edite o arquivo .env e altere os parametros de acesso ao banco de dados

```dockerfile
DB_DATABASE=laravel
DB_USERNAME=root
DB_PASSWORD=lara_root
```

Execute o migrate

```bash
php artisan migrate
```

```bash

```
