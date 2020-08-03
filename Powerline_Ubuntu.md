# Powerline no Ubuntu

Instalando os pacotes

```bash
sudo apt install fonts-cascadia-code git wget
```

Preparando para ativer o Powerline

```bash
mkdir /usr/local/bin/powerline
```

```bash
git clone https://github.com/powerline/powerline.git /usr/local/bin/powerline/
```

```bash
wget https://github.com/powerline/powerline/raw/develop/font/PowerlineSymbols.otf -O /usr/share/fonts/PowerlineSymbols.otf
```

```bash
wget https://github.com/powerline/powerline/raw/develop/font/10-powerline-symbols.conf -O /etc/fonts/conf.d/10-powerline-symbols.conf
```

Pronto, agora execute o comando para ativar o powerline quando vc logar no terminal.

```bash
echo '. /usr/local/bin/powerline/powerline/bindings/bash/powerline.sh' >> ~/.bashrc
```

### No VSCode

Edite o settings.json e mude as seguintes linhas:

```json
"editor.fontFamily": "'Cascadia Code PL', 'Droid Sans Mono', 'monospace', monospace, 'Droid Sans Fallback'",
"editor.fontLigatures": true,
```

E logo abaixo adicione a seguinte linha:

```json

```
