# Procedimento de nova instancia AWS

Informativo para processo a ser executado após criação de nova instancia na AWS.

Para criar uma nova instancia siga este documento: [Create an Amazon EC2 instance](https://docs.aws.amazon.com/codedeploy/latest/userguide/instances-ec2-create.html)

> Utilizamos como imagem EC2 padrão o Ubuntu 20.04 64bits

Com acesso a sua chave conecte-se a nova instancia

```bash
ssh -i nomedesuachave.pem ubuntu@ip_publico_sua_instancia
```

## Atualização do sistema & ferramentas iniciais

```bash
sudo apt update &&\
sudo apt -y upgrade &&\
sudo apt install -y ruby wget vim apt-transport-https ca-certificates curl software-properties-common postgresql-client-common postgresql-client
```

## Cliente CodeDeploy

```bash
wget https://aws-codedeploy-us-east-2.s3.us-east-2.amazonaws.com/latest/install &&\
chmod +x install &&\
sudo ./install auto > /tmp/logfile &&\
sudo service codedeploy-agent status
```

## Instalação do Docker

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - &&\
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable" &&\
sudo apt update &&\
sudo apt install -y docker-ce &&\
sudo systemctl status docker &&\
sudo usermod -aG docker ${USER} &&\
```

Aqui recomenda-se que saia de sua sessão e conecte-se novamente.

Docker-compose

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.26.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose &&\
sudo chmod +x /usr/local/bin/docker-compose &&\
docker-compose --version
```

## Chaves SSH para serviços GIT/CodeCommit

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

Adicione o conteúdo de sua chave ao IAM da AWS. 

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

Adiciona a Iniclização do SSH-AGENT em eu `.bashrc`

```bash
vim .bashrc
```

Cole este conteúdo (Não esqueça de apontar o arquivo de chave correto):

```bash
if [ -z "$SSH_AUTH_SOCK" ] ; then
  eval `ssh-agent -s`
  ssh-add ~/.ssh/chavesegura_profissional.pub
fi
```

Também adicione a finalização em seu `.bash_logout`

```bash
vim .bash_logout
```

Cole este conteúdo:

```bash
if [ -n "$SSH_AUTH_SOCK" ] ; then
  eval `/usr/bin/ssh-agent -k`
fi
```

Pronto... se foi feito tudo certo, agora vc deve acessar todos seus repositorios via SSH. 

E finalize adicionando sua chave ao gereciador do sistema (Repita este comando apontando para cada chave que queria utilizar).

```bash
ssh-add ~/.ssh/chavesegura_profissional.pub
```

## Clonar repositorio

Em breve

## Adicionar tarefa ao cron para atualização de branch

Em breve
