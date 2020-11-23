# CodeCommit como REPO de Homolog via chave SSH em um REPO Developer do Bitbucket.

Aqui eu utilizo chaves ssh, tanto para o bitbucket quanto para o CodeCommit. Então primeiro vou mostrar como configurar este tipo de acesso em ambos os ambientes.

Primeiro, criamos nossa chave. Caso vc já tenha a sua por favor pule este passo. 

De acordo com o artigo [Secure Secure Shell](https://stribika.github.io/2015/01/04/secure-secure-shell.html) a forma mais segura em 2020 de ter uma chave encripitada é executando o ssh-keygen utilizando os parametros abaixo.

```bash
ssh-keygen -t ed25519 -a 100
```

Mas se vc tem uma abordagem mais conservadora e não tão moderna você pode gerar sua chave mais simples mesmo.

```bash
ssh-keygen -t rsa -b 4096 -o -a 100
```

Lembre-se de dar um nome diferente para esta chave. Afinal ela não será de uso pessoal. 

Copie sua chave publica:

```bash
cat ~/.ssh/chavesegura_profissional.pub
```

Para aplicar sua chave no bitbucket acesse https://bitbucket.org/account/settings/ssh-keys/  Insira sua chave seguindo  as instruções.

No caso da AWS é necessário um passo a mais. Acesse o console AWS, vá até o IAM, em gerenciamento de acesso, vá até usuários e selecione seu usuário na lista. 

Agora acesse a aba credenciais de segurança e clique sobre o botão "Fazer Upload de chave publica SSH"

Agora vem aquele passo extra! Com sua chave adicionada, copie o "ID da chave do SSH" e crie um arquivo. 

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

Espero ter ajudado. 
