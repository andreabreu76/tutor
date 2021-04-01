# WSL 2 - Developer Ambient

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
sudo apt install -y php7.3-common \
php7.3-fpm \
php7.3-gd \
php7.3-mysql \
php7.3-curl \
php7.3-intl \
php7.3-mbstring \
php7.3-bcmath \
php7.3-imap \
php7.3-xml \
php7.3-zip \
libmcrypt-dev \
php-tokenizer \
libmagickwand-dev
```

Install on ubuntu

```bash
sudo apt install -y php7.4-common \
php7.4-fpm \
php7.4-gd \
php7.4-mysql \
php7.4-curl \
php7.4-intl \
php7.4-mbstring \
php7.4-bcmath \
php7.4-imap \
php7.4-xml \
php7.4-zip \
libmcrypt-dev \
php-tokenizer \
libmagickwand-dev
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
#### AWS Cli v2

Instalação, em seu terminal cole o comando:

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" && \
    unzip awscliv2.zip && \
    sudo ./aws/install
```

Verifique a instalação

```bash
aws --version
```

#### Kubectl

instalação, em seu terminal cole o comando:

```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" && \
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

Verifique a instalação 

```bash
kubectl version --client
```
#### Configurando o Git 

Setando as configurações globais

```bash
git config --global user.name "<Your-Full-Name>"
```
```bash
git config --global user.email "<your-email-address>"
```

Uns opcionais

```bash
git config --global color.ui auto
```
```bash
git config --global merge.conflictstyle diff3
```
```bash
git config --global core.editor "code --wait"
```

Verifique se está tudo correto:

```bash
git config --list
```

#### Chaves SSH para serviços GIT/CodeCommit/BitBucket

Aqui eu utilizo chaves ssh, tanto para o bitbucket quanto para o CodeCommit ou GitHub. Então primeiro vou mostrar como configurar este tipo de acesso em ambos os ambientes.

Primeiro, criamos nossa chave. Caso vc já tenha a sua por favor pule este passo ou siga as instruções do comando.

> Lembre-se que se trata de acesso confidencial, eu utilizo este gerador de senhas aqui [Keygenerator - Lastpass](https://www.lastpass.com/pt/password-generator). E utilizo 8 caracteres de fácil memorização. 
 
```bash
ssh-keygen -t rsa -b 4096 -o -a 100
```

> Dê um nome diferente para esta chave. Afinal ela não será de uso pessoal. 

Copie sua chave publica com o comando:

```bash
cat ~/.ssh/chavesegura_profissional.pub
```

Para aplicar sua chave no bitbucket acesse https://bitbucket.org/account/settings/ssh-keys/  Insira sua chave seguindo  as instruções.

No caso da AWS é necessário um passo a mais. 

- Acesse o console AWS, vá até o IAM, em gerenciamento de acesso, vá até usuários e selecione seu usuário na lista. 
- Agora acesse a aba credenciais de segurança e clique sobre o botão "Fazer Upload de chave publica SSH"
- Agora vem aquele passo extra! Com sua chave adicionada, copie o "ID da chave do SSH" e crie um arquivo. 

```bash
nano -w ~/.ssh/config
```

Nele você adiciona as seguintes linhas:

```javascript
Host git-codecommit.*.amazonaws.com
  User <SEU_ID_DE_CHAVE_COPIADO>
  IdentityFile ~/.ssh/chavesegura_profissional.pub
```

Pronto... se foi feito tudo certo, agora vc deve acessar todos seus repositorios via SSH. 

Agora vamos ao multi repositorio.

Digamos que seu ambiente de desenvolvimento seja MICROSERVICO e que vc fez o git clone de um REPO no Bitbucket. Então ao digitar.

```bash
git remote -v
```

 Sua resposta será algo como:

```bash
origin  git@bitbucket.org:meuusuario/microservico.git (fetch)
origin  git@bitbucket.org:meuusuario/microservico.git (push)
```

E o que precisamos fazer é adicionar um novo repositorio com um alias organizacional para que tenhamos dois no nosso serviço. Então para isso o comando é:

```bash
git remote add homolog ssh://git-codecommit.sg-world-38.amazonaws.com/v1/repos/microservicos
```

Agora o resultado do comando "git remote -v" será algo como:

```bash
homolog ssh://git-codecommit.sg-world-38.amazonaws.com/v1/repos/microservico (fetch)
homolog ssh://git-codecommit.sg-world-38.amazonaws.com/v1/repos/microservico (push)
origin  git@bitbucket.org:meuusuario/microservico.git (fetch)
origin  git@bitbucket.org:meuusuario/microservico.git (push)
```

Então: 

- Todo pull feito em *origin* irá puxar do REPO Bitibucket.
- Todo push feito em *origin* irá publicar no REPO Bitbucket
- Todo pull feito em *homolog* irá puxar do REPO CodeCommit
- Todo push feito em *homolog* irá publicar no REPO CodeCommit

Após o *CODACY* aprovar sua pull-request de alteração no Bitbucket nós devemos fazer o pull da branch `develop` do bitbucket para só então fazer o push no homolog.  

```bash
git pull origin develop
```

Só então fazer o push para homolog.

```bash
git push homolog develop
```
