# Powerline - WSL ou Sistema Linux de forma simples.

Primeiro caso esteja utilizando WSL instale as devidas fontes em seu sistema Windows. A mais popular é um projeto opensource criada pela propria Microsofit e pode ser adquirida aqui [Cascadia Code PL](https://github.com/microsoft/cascadia-code/releases) Eu a utilizo no WSL e mesmo no sistem Linux puro também. 

instale as fontes em sua distro WSL ou sistema Linux também.

```bash
sudo apt install fonts-powerline fonts-cascadia-code fonts-firacode
```

Caso a instalação do pacote <mark>fonts-cascadia-code</mark> dê erro é porque ele não encontra-se em seu repositorio. Então devemos proceguir com a instalação manual. 

Baixe o pacote:

```bash
wget http://ftp.br.debian.org/debian/pool/main/f/fonts-cascadia-code/fonts-cascadia-code_2005.15-1_all.deb
```

Agora instale ele:

```bash
sudo dpkg -i fonts-cascadia-code_2005.15-1_all.deb
```

Agora vamos a instalação do powerline

```bash
sudo apt install -y powerline powerline-gitstatus python3-powerline vim-airline
```

Após a instalação vamos ativar o powerline em seu BASH simplestmente pondo as linhas a seguir ao fim do seu arquivo $HOME/.bashrc

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
