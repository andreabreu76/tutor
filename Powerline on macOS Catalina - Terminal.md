# Powerline on macOS Catalina - Terminal

Install Tool's 

```bash

```

Install Xcode Commando tools

```bash
xcode-select --install
```

Install Homebrew

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)" 
```

Verifica se Python está instalado

```bash
which python3
```

Deve retornar algo como /usr/bin/python3

Agora instalemos o PIP

```bash
sudo easy_install pip
```

Feito isso teste o comando e atualize ele. 

```bash
sudo pip install --upgrade pip
```

```bash
python --version && pip --version
```

Então o resultado deve ser ...

> Python 2.7.16

> pip 20.2.3 from /Library/Python/2.7/site-packages/pip-20.2.3-py2.7.egg/pip (python 2.7)

## Dando Inicio a instalação

Install  Powerline

```bash
pip install --user powerline-status
```

Execute

```bash
/Users/andreabreu/Library/Python/2.7/lib/python/site-packages/powerline/bindings/zsh/powerline.zsh
```

Se a executar o powerline houver erro de "file not found" beixe o repositorio e faça o download das partes que faltam. 

[GitHub - powerline/powerline: Powerline is a statusline plugin for vim, and provides statuslines and prompts for several other applications, including zsh, bash, tmux, IPython, Awesome and Qtile.](https://github.com/powerline/powerline)


