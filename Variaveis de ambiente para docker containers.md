# Variaveis de ambiente para docker containers

As variaveis a serem declaradas são geradas dentro do proprio container, sendo assim, é necessario um script que rode na inicialização do container. Essa demanda também impede que as variaveis de ambiente seja declaradas no docker-compose.yml na sessão envirioment.

O utilitario que ira gerar os dados que precisamos definir como variaveis de ambiente também precisa que o container de banco de dados esteja ativo e funcionando, daí nossa necessidade de utilizar um script de terceiros bem conhecido na comunidade Docker. O [wait-for-it](https://github.com/vishnubob/wait-for-it),  mencionado em [Docker Documentation](https://docs.docker.com/compose/startup-order/). Para que nosso script seja executado somente com a confirmação de suas dependencias.

Então segue as alterações:

### app-dockerfile

Adicionei o utilitario `WGET` a linha que executa a instalação das dependencias do PHP para o Container, não deixe da faze-lo para que o utlilitario seja baixado. E ao fim do arquivo adicionei as linhas que nos trará o utilitario wait-for-it. Também defino aqui as permissões de execução tanto do utilitario quando do script. 

```dockerfile
RUN wget -O /usr/local/bin/wait-for-it.sh https://raw.githubusercontent.com/vishnubob/wait-for-it/8ed92e8cab83cfed76ff012ed4a36cef74b28096/wait-for-it.sh \
    && chmod +x /usr/local/bin/wait-for-it.sh

CMD cd /var/www/ && chmod +x passportGen.sh 
```

### docker-compose.yml

É onde acionaremos nosso script a ser colocado no diretorio de montagem. Por isso devemos colocar as seguintes linhas apos o parametro `volumes `dentro do `app`

```docker
depends_on:
    - "database"
command: ["/usr/local/bin/wait-for-it.sh", "database:3306", "--", "/var/www/passportGen.sh"]
```

### Script (passportGen.sh)

No diretorio declarado em volumes do arquivo `docker-compose.yml`, pode ser criado fora ou dentro do conteiner. Iremos adicionar o scrtip.

```bash
#!/usr/bin/env bash

## (-e) SAIDA QUANDO ENCONTRA ERROS, UTILIZE O (-x) PARA DEBUG 
set -x

[[ -f "/tmp/result.tmp" ]] && eval "rm /tmp/result.tmp"

eval "cd /var/www/ && php artisan passport:install >> /tmp/result.tmp"

getId=$(cat /tmp/result.tmp | grep -e "Client ID" -m 1 | awk '{print $3}')
getSecret=$(cat /tmp/result.tmp | grep -e "Client secret" -m 1 | awk '{print $3}')

if [ -f "/var/www/.env" ]; then

    idComand="sed -i '/PASSPORT_CLIENT_ID/c\PASSPORT_CLIENT_ID=$getId' /var/www/.env"
    secretComand="sed -i '/PASSPORT_CLIENT_SECRET/c\PASSPORT_CLIENT_SECRET=$getSecret' /var/www/.env"

else

    idComand="echo 'PASSPORT_CLIENT_ID=$getId' >> /var/www/.env"
    secretComand="echo 'PASSPORT_CLIENT_SECRET=$getSecret' >> /var/www/.env"

fi

eval $idComand
eval $secretComand
eval "tail -f /dev/null"
```

Explicando...

... `set -e` define que caso haja um erro, o script para imediatamente. Se o parametro "-e" for substituido pelo "-x" todo o script tera sua saída printada na tela servindo como debug.

Testo a existencia da saida do comando que ira gerar os dados que precisamos, isso garante que este arquivo sempre terá a ultima saída. 

Executo o comando e em seguida capturo filtrando a saída do comando para obter somente os dados que preciso

Gero um arquivo a ser utilizado pelo sistema. 
