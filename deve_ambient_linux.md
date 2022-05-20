# Ambiente Fullstack Laravel & Vue no Ubuntu 20.04

#### Basics

```bash
sudo apt update && sudo apt upgrade
```

#### Essentials

```bash
sudo apt install -y vim wget curl git ssh unzip
```

#### Node.js

```bash
sudo curl -sL https://deb.nodesource.com/setup_16.x | sudo -E bash -
```

```bash
sudo apt install -y nodejs
```

Check install:

```bash
node -v && npm -v
```

Se ocorreu tudo bem ele deve mostrar algo como:

```shell
v14.7.0
6.14.7
```

#### PHP

```bash
sudo apt install php7.4-common php7.4-fpm php7.4-gd php7.4-mysql php7.4-curl php7.4-intl php7.4-mbstring php7.4-bcmath php7.4-imap php7.4-xml php7.4-zip libmcrypt-dev php-tokenizer libmagickwand-dev
```

Check install:

```bash
php --version && php-fpm7.4 --version
```

Se ocorreu tudo bem o resultado deve ser algo como:

```shell
PHP 7.4.3 (cli) (built: May 26 2020 12:24:22) ( NTS )
Copyright (c) The PHP Group
Zend Engine v3.4.0, Copyright (c) Zend Technologies
    with Zend OPcache v7.4.3, Copyright (c), by Zend Technologies
PHP 7.4.3 (fpm-fcgi) (built: May 26 2020 12:24:22)
Copyright (c) The PHP Group
Zend Engine v3.4.0, Copyright (c) Zend Technologies
    with Zend OPcache v7.4.3, Copyright (c), by Zend Technologies
```

#### Composer

```bash
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
```

```bash
sudo php composer-setup.php --install-dir=/usr/local/bin/ --filename=composer
```

Para comfirmar digite:

```bash
composer --version
```

E veja:

```bash
Composer version 1.10.9 2020-07-16 12:57:00
```

#### Laravel

```bash
composer global require laravel/installer
```

Adicione a linha seguinte em seu .bashrc

```bash
export PATH="$PATH:$HOME/.config/composer/vendor/bin"
```

E teste com o comando:

```bash
laravel --version
```

E o resultado deve ser:

```bash
Laravel Installer 3.2.0
```

### Teste Pratico

Crie um novo projeto laravel...

```bash
laravel new testblog
```

```bash
cd testblog
```

Instale os componentes...

```bash
composer install
```

Verifique se há atualizações no node.js

```bash
sudo npm i npm -g
```

Instale os pacotes node.js

```bash
npm install 
```

Instale o sistem de interface do usuario... 

```bash
composer require laravel/ui
```

Defina o framework do frontend...

```bash
php artisan ui vue
```

Instale a UI do seu framework...

```bash
php artisan ui vue --auth
```

Mais algumas dependencias ... 

```bash
npm install && npm run dev
```

Rode a instancia ... 

```bash
php artisan serve
```

E Voilá

http://localhost:8000

Ao tentar se cadastrar resultará em erro pois você precisa de um Banco de Dados. Acesse este LINK para configurar e rodar um container Docker com  banco de dados MariaDB. 
