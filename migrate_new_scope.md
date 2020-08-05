# Adicionado migrate para novo scope

O controle de scope é feito para aumentar a segurança de ocmunicação entra os micro serviços.  

´´´bash
docker exec -it porteira_app_1 php artisan make:migration update_affiliate_scope_pass_by_admin_controller --table=scopes
´´´

´´´bash
docker exec -it porteira_app_1 php artisan make:migration update_affiliate_role_pass_by_admin_controller --table=roles_scopes
´´´

