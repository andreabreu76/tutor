# WSL 2 - Developer Ambient with Debian

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
sudo curl -sL https://deb.nodesource.com/setup_14.x | sudo bash -
```

```bash
sudo apt install -y nodejs
```

Check install:

```bash
node -v && npm -v
```

#### PHP

Install on Debian

```bash
sudo apt install php7.3-common php7.3-fpm php7.3-gd php7.3-mysql php7.3-curl php7.3-intl php7.3-mbstring php7.3-bcmath php7.3-imap php7.3-xml php7.3-zip libmcrypt-dev php-tokenizer libmagickwand-dev
```

Install on ubuntu

```bash
sudo apt install php7.4-common php7.4-fpm php7.4-gd php7.4-mysql php7.4-curl php7.4-intl php7.4-mbstring php7.4-bcmath php7.4-imap php7.4-xml php7.4-zip libmcrypt-dev php-tokenizer libmagickwand-dev
```

Check install (Debian or Ubuntu):

```bash
php --version && php-fpm7.3 --version
```

```bash
php --version && php-fpm7.4 --version
```


#### Composer

```bash
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
```

```bash
sudo php composer-setup.php --install-dir=/usr/local/bin/ --filename=composer
```

#### Laravel

```bash
composer global require laravel/installer
```

Add next line on your .bashrc end file. 

```bash
export PATH=$PATH:$HOME/.composer/vendor/bin
```
