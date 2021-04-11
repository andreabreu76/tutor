# Powerline - WSL2 ou Sistema Linux de forma simples.

Primeiro caso esteja utilizando WSL instale as devidas fontes em seu sistema Windows. A mais popular é um projeto opensource criada pela propria Microsofit e pode ser adquirida aqui [Cascadia Code PL](https://github.com/microsoft/cascadia-code/releases) ou a criada pelo Mozila que é muito bem vinda para monitores UHD, que é a [Fira Code](https://github.com/tonsky/FiraCode). Eu a utilizo no Windows com WSL2 e mesmo no sistema Linux puro também. 

Com as fontes instaladas no seu Desktop Windows vamos deixar pronto seu  [Terminal Windows](https://www.microsoft.com/pt-br/p/windows-terminal/9n0dx20hk701?activetab=pivot:overviewtab).  Após instalar o app do terminal, clique na setinha para baixo ao lado da Aba e depois em configurações.  Será aberto seu editor de JSON padrão. 

O indice `Profiles` haverá o sub-indice `list` e nele as distros que você tem instalado. Copie o valor de `guid` da distro que você mais utiliza e cole no valor de `"defaultProfile"` . 

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
