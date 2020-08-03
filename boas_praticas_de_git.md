# Boas praticas de git

Recentemente ingressei em um ambiente de desenvolvimento gigantesco e muito importante. Com isso as boas praticas de GIT tanto para linha de comando quando para extenções na sua IDE de programação são essenciais. 

Primeiramente nunca altere nada, nenhum arquivo que não seja diretamente ligado a sua tarefa, evite alterar funções existentes, renomea-las ou qualquer coisa do tipo, sou programador PHP e por vezes mudar a ordem de uma linha pode afetar todo o projeto. 

Agora vamos ao WORKFLOW.

### Ferramentas essenciais

Primeiro confirme se há as ferramentas necessárias instaladas em seu sistema.

```bash
sudo apt update && sudo apt -y upgrade && sudo apt install -y ssh git wget curl
```

### Preparando o ambiente

O segundo passo é gerar uma chave SSH para trazer todos seus projetos do HUB de forma segura. 

Vamos gerar a sua chave, Há e não deixa-a sem uma senha:

```bash
ssh-keygen -t rsa
```

Com sua chave gerada haverá uma pasta em seu HOME chamada .ssh copie a chave publica. 

```bash
cat ~/.ssh/id_rsa.pub
```

Copie o resultado deste comando e abra o site de seu repositorio, nesse ponto vc deve ter obitido o acesso com seu gerente ou supervisor. No github por exemplo clique em sua foto no canto superior esquerdo e depois vá até Settings, no menu direito havera o SSH and GPG Keys. Clique em  Add SSH Key, dê um nome e cole o conteúdo, feito isso salve.   No bitbucket o menu fica no canto inferior esquerdo, clique sob sua foto e em seguida em Personal Settings, no menu Securety haverá o submenu SSH Keys. 

### Baixando um projeto

Com sua chave SSH devidamente configurada basta que clone o repositorio em suas arvore de diretorio organizada. No meu exemplo eu criei em meu home uma pasta chamada codigos para alocar todos os micros serviços, mais a frente explicarei a ventagem de se fazer assim. 

```bash
cd codigos/
```

```bash
git clone git@github.com:seuNomedeUsuario/seuProjeto.git
```

### Finalmente, iniciado seu trabalho.

Definir as variasi de ambiente do git

```bash
git config --global user.name "Seu Nome"
git config --global user.email seu-email@empresa.com
```

Com tudo pronto vamos ao trabalho.

```bash
cd seuProjeto/
```

Garanta que esteja tudo atualizado.

```bash
git checkout master && git pull
```

Crie sua nova branch, As Branch's são muito importantes para versionamento. Não deixe de verificar com seu gestor se há regras de nomeclatura.

```bash
git checkout [sua-nova-branch] 
```


