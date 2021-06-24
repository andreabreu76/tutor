# Instalando WSL2 no Windows

Este é um resumo objetivo do que tem escrito neste tutorial oficial. [https://docs.microsoft.com/pt-br/windows/wsl/install-win10](https://docs.microsoft.com/pt-br/windows/wsl/install-win10)

Habilite o subsistema Linux no Windows

```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

Habilite o recurso de maquina virtual

```powershell
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

Instale o suporte a WSL2

[https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)

Defina o WSL2 como opção padrão

```powershell
wsl --set-default-version 2
```

Agora instale o Ubuntu 20.04 LTS (Ultima versão estavel)

[Ubuntu 20.04 LTS - Microsoft Store](https://www.microsoft.com/en-us/p/ubuntu-2004-lts/9n6svws3rx71)

Feita a intalação, execute o App. Um terminal será abero e irá solicitar que configura um usuário e senha.

Feito isso feche-o e faça o download do [Windows Terminal](https://www.microsoft.com/en-us/p/windows-terminal/9n0dx20hk701)

Pronto, com seu user logado em seu Subsistema Linux no Windows vamos a preparação do ambiente de desenvolvimento.

## WSL 2 - Developer Ambient

### Auth

```bash
sudo su -
```

Configure uma senha forte. Você utilizará sempre o sudo, mas com vamos ir além de simples comandos. Algo pode dar errado e com esse login ativado, podemos reverter facilmente

```bash
passwd
```

Agora uma facilidade, adicione o grupo ao nopasswd. Lembrando que isso é recomendavel somente caso vc esteja utilizando um desktop ou dispositivo mais seguro.

```bash
echo "%adm  ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers.d/adm.sudoers
```

```bash
exit
```

Pronto  agore seguieremos.

### Basics

```bash
sudo apt update && sudo apt upgrade
```

### Essentials

```bash
sudo apt install -y vim wget curl git ssh unzip
```

### Node.js

```bash
sudo curl -sL https://deb.nodesource.com/setup_16.x | sudo bash -
```

```bash
sudo apt install -y nodejs
```

Check install:

```bash
node -v && npm -v
```

### Configurando o Git

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

### Chaves SSH para serviços Github

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

Agora acesse sua conta no Github clique no seu icone de profile, selecione Settings e depois vá até `SSH and GPG Keys` no lado direito selecione `New SSH Key` cole sua chave publica copiada, dê um nome e pronto.

## Docker

PAra instalaro  Docker com suporte WSL2 basta seguir os passos

Faça download do [Docker desktop](https://docs.docker.com/docker-for-windows/wsl/#download) para Windows.

Feito isso, acesse as configurações, clicando no icone do Tray proximo ao relogios e selecionando Settings, depois escolha Resource e finalmente em WSL integration. Ao lado deve aparecer sua distro Linux instalada, no nosso caso `Ubuntu 20.04 LTS` habilite e reinicie.

Após reiniciar, abra seu terminal e digite o comando:

```bash
sudo usermod -aG docker ${USER}
```

Agora de forma totalmente opicional vamos a cereja do bolo.

## Powerline - WSL2 ou Sistema Linux de forma simples

Primeiro caso esteja utilizando WSL instale as devidas fontes em seu sistema Windows. A mais popular é um projeto opensource criada pela propria Microsofit e pode ser adquirida aqui [Cascadia Code PL](https://github.com/microsoft/cascadia-code/releases) ou a criada pelo Mozila que é muito bem vinda para monitores UHD, que é a [Fira Code](https://github.com/tonsky/FiraCode). Eu a utilizo no Windows com WSL2 e mesmo no sistema Linux puro também.

Com as fontes instaladas no seu Desktop Windows vamos deixar pronto seu  [Terminal Windows](https://www.microsoft.com/pt-br/p/windows-terminal/9n0dx20hk701?activetab=pivot:overviewtab).  Após instalar o app do terminal, clique na setinha para baixo ao lado da Aba e depois em configurações.  Será aberto seu editor de JSON padrão.

O indice `Profiles` haverá o sub-indice `list` e nele as distros que você tem instalado. Copie o valor de `guid` da distro que você mais utiliza e cole no valor de `"defaultProfile"`.

Depois vá em `defaults` e sugiro por o seguinte:

```json
// Put settings here that you want to apply to all profiles.
"fontFace": "Fira Code Retina",
"fontSize": 11,
"cursorShape": "underscore",
"useAcrylic": true,
"acrylicOpacity": 0.7,
"bellStyle":"visual",
"historySize":19001,
"antialiasingMode": "cleartype",
"altGrAliasing": true,
"closeOnExit":"graceful",
"initialCols":120
```

Outra facilidade que sugiro é que no indice de sua distro preferida você ponha o valor:

```json
"startingDirectory": "\\\\wsl$\\Ubuntu-20.04\\home\\seunomedeusuario"
```

Agora vamos instalar em sua distro Linux no WSL2.

```bash
sudo apt install fonts-powerline fonts-cascadia-code fonts-firacode
```

Caso a instalação do pacote dê errado pode ser que seu sistema não tenha repositorios dela, sugiro olhar [aqui](https://github.com/microsoft/cascadia-code) para Casacadia e [aqui](https://github.com/tonsky/FiraCode) para Fire.

Agora vamos a instalação do powerline

```bash
sudo apt install -y powerline powerline-gitstatus python3-powerline vim-airline
```

Após a instalação vamos ativar o powerline em seu BASH simplestmente pondo as linhas a seguir ao fim do seu arquivo ~/.bashrc

```bash
# Powerline configuration
if [ -f /usr/share/powerline/bindings/bash/powerline.sh ]; then
  powerline-daemon -q
  POWERLINE_BASH_CONTINUATION=1
  POWERLINE_BASH_SELECT=1
  source /usr/share/powerline/bindings/bash/powerline.sh
fi
```

Para aplicar na sua atual sessão, execute:

```bash
source ~/.bashrc
```

## Powerline no Powershell do Windows

Primeiro abra o powershell em modo administrador. E execute o comando

```powershell
Set-ExecutionPolicy Unrestricted
```

Isso fará com que os modulos não tenham problemas em ser instalados.

Para instalar os modulos, você precisa acessar o powershell sem ser administrador.

```powershell
Install-Module posh-git -Scope CurrentUser
```

```powershell
Install-Module oh-my-posh -Scope CurrentUser
```

```powershell
Install-Module -Name PSReadLine -Scope CurrentUser -Force -SkipPublisherCheck
```

Pronto, com tudo instalado vamos iniciar os modulos a cada vez que vc abrir um powershell. Para isso execute:

```powershell
notepad $PROFILE
```

Se ele disse que o arquivo não existe, pode cria-lo sem problemas e cole as proximas linhas nele.

```textile
Import-Module posh-git
Import-Module oh-my-posh
Set-PoshPrompt -Theme powerlevel10k_lean
```

Salve e reinicie seu terminal.

Para ter uma experiencia visual melhor, instale a fonte Cascadia Code PL e nas configurações do Windows Terminal utilize estes parametros

```json
{
    // Make changes here to the powershell.exe profile.
    "guid": "{61c54bbd-c2c6-5271-96e7-009a87ff44bf}",
    "name": "Windows PowerShell",
    "commandline": "powershell.exe",
    "fontFace": "Cascadia Code PL",
    "hidden": false
},
```

Caso queira experimentar novos visuais para seu prompt execute o comando:

```powershell
Get-PoshThemes
```

E no seu arquivo de inicialização `notepad $PROFILE` altere a linha `Set-PoshPrompt -The <nome_do_novo_tema>`
