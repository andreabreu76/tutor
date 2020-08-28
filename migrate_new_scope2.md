# Adicionado migrate para novo scope

O controle de scope é feito para aumentar a segurança de comunicação entre os micro serviços.  

Para criar o scope digite o comando:

```bash
docker exec -it [nome_do_container] php artisan make:migration [titulodafuncao]_scope_controller --table=scopes
```

Como resultado será criado o arquivo database/migrates com o final  [titulodafuncao]_scope_controller.php

Edite adicionando o seguinte na sessão schema da função UP substitua a linha comentada pelo seguinte conteúdo, atente-se para o correto preenchimento:

```php
DB::table("scopes")->insert(
    array(
        array('scope' => '[titulodafuncao]_scope_controller',
        'description' => 'DESCRICAO DA FUNCAO',
        'path' => '/api/[PATH DA API DECLARADA EM api.php]') 
        //Manter "/api/" e retirar a ultima "/"
    )
);
```

```bash
docker exec -it [nome_do_container] php artisan make:migration [titulodafuncao]_role_controller --table=roles_scopes
```

Como resultado será criado o arquivo database/migrates com o final [titulodafuncao]_role_controller.php

Edite adicionando o seguinte na sessão schema da função UP substitua a linha comentada pelo seguinte conteúdo, alterando o LIKE para o titulo do scope. 

```php
DB::insert("insert into roles_scopes (role_id, scope_id)
select t1.id,t2.id from
(select roles.id from homestead.roles where roles.id IN (1)) as t1,
(select scopes.id from homestead.scopes where scopes.scope LIKE ('update_affiliate_scope_pass_by_admin')) as t2");
```
