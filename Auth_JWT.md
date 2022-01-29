# API de Autenticação com Laravel e JWT

Este é mais um tutorial de fixação baseado no Windows 10 e utilizando no ambiente Windows não em WSL por tanto utilizaremos o PowerShell.

## Instalando o PHP no Powershell

Faça o download da ultima versão estável do php em https://php.net.  Descompacte o arquivo baixado em um local de sua preferencia e adicione no Path do sistema. No meu caso eu descompactei o PHP8 no diretório `C:\php8` então abri o dialogo do Windows, *Propriedades do Sistema* cliquei no botão  *Variáveis de Ambiente* e no dialogo que se abriu eu selecionarei PATH do espaço *Variáveis do Sistema* depois cliquei em *Editar* e então em *Novo* e adicionei `C:\php8` apliquei as mudanças e fechei todos as caixas de dialogo do Windows.

Feito isso podemos verificar a versão em um novo powershell
```powershell
php --version
```
E o resultado deve ser
```powershell
PHP 8.1.2 (cli) (built: Jan 19 2022 10:13:52) (NTS Visual C++ 2019 x64)
Copyright (c) The PHP Group
Zend Engine v4.1.2, Copyright (c) Zend Technologies                     
```
Pronto, agora daremos sequencia com o composer. Com o instalador mais simples basta verificar após instala-lo.
```powershell
composer --version
```
E o resultado deve ser
```powershell
Composer version 2.2.5                     
```

## Inicio do projeto

No powershell em um diretório de minha presencia executarei o comando que criará uma base Laravel de nosso projeto. 

```powershell
composer create-project laravel/laravel laravelJwtAuth --prefer-dist
```
Terminada a instalação, acesso o arquivo `.env` com o editor de minha preferencia e configuro os parâmetros de acesso ao meu banco de dados que no caso é um MySQL.

```json
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravelJwtAuth
DB_USERNAME=root
DB_PASSWORD=
```
Vale lembrar que o database deve ser existente porque embora o migration do Laravel crie tabelas e manipule bem essa parte, ele não cria o database. Então se você não tiver um database, acesse seu programa preferido e crie um. 

Após salvar faremos um reload das configurações, Estando no diretório do projeto no meu caso em `C:\Servicos\laravelJwtAuth` eu executarei o comando

```powershell
php artisan config:clear
```
```powershell
php artisan migrate
```
Assim o Laravel já criará as tabelas de controle do migration assim como as de usuários.

Agora instalaremos um pacote de terceiros, o https://github.com/tymondesigns/jwt-auth que nos permitirá que utilizemos a autenticação por json web token (JWT).  

```powershell
composer require tymon/jwt-auth:*
```
Agora com o JWT instalado na parta VENDOR do Laravel, vamos adicionar o provedor de serviços no array de PROVIDERS no arquivo `config/app.php` e também declararemos os FACADES no array de aliases. 

```php
'providers' => [
    ....
    ....
    Tymon\JWTAuth\Providers\LaravelServiceProvider::class,
],
'aliases' => [
    ....
    'JWTAuth' => Tymon\JWTAuth\Facades\JWTAuth::class,
    'JWTFactory' => Tymon\JWTAuth\Facades\JWTFactory::class,
    ....
],
```
Agora publicaremos o vendor
```powershell
php artisan vendor:publish --provider="Tymon\JWTAuth\Providers\LaravelServiceProvider"
```
E então geraremos a chave em nosso .env

```powershell
php artisan jwt:secret
```
Enfim, alteraremos a model User em `app/Models/User.php` substituindo seu conteúdo por este.

```php
<?php

namespace App\Models;

use Illuminate\Contracts\Auth\MustVerifyEmail;
use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Foundation\Auth\User as Authenticatable;
use Illuminate\Notifications\Notifiable;

use Tymon\JWTAuth\Contracts\JWTSubject;


class User extends Authenticatable implements JWTSubject
{
    use HasFactory, Notifiable;

    /**
     * The attributes that are mass assignable.
     *
     * @var array
     */
    protected $fillable = [
        'name',
        'email',
        'password',
    ];

    /**
     * The attributes that should be hidden for arrays.
     *
     * @var array
     */
    protected $hidden = [
        'password',
        'remember_token',
    ];

    /**
     * The attributes that should be cast to native types.
     *
     * @var array
     */
    protected $casts = [
        'email_verified_at' => 'datetime',
    ];
    
    /**
     * Get the identifier that will be stored in the subject claim of the JWT.
     *
     * @return mixed
     */
    public function getJWTIdentifier() {
        return $this->getKey();
    }

    /**
     * Return a key value array, containing any custom claims to be added to the JWT.
     *
     * @return array
     */
    public function getJWTCustomClaims() {
        return [];
    }    
}
```

Agora, precisamos configurar o JWT Auth Guard para proteger o processo de autenticação do aplicativo Laravel. Adicione estes parâmetros no arquivo `config/auth.php`

```php
<?php

return [

    'defaults' => [
        'guard' => 'api',
        'passwords' => 'users',
    ],


    'guards' => [
        'web' => [
            'driver' => 'session',
            'provider' => 'users',
        ],

        'api' => [
            'driver' => 'jwt',
            'provider' => 'users',
            'hash' => false,
        ],
    ],
```

Nesta etapa, criaremos a Controller de autenticação JWT e, nesta Controller de autenticação, definiremos a lógica principal para o processo de autenticação seguro no Laravel 8.

Vamos definir o controlador de autenticação manualmente ou usando o comando abaixo para gerenciar as solicitações de autenticação por meio de rotas que criaremos.

```powershell
 php artisan make:controller AuthController
```
Esta Controller deverá ter o seguinte conteúdo

```php
<?php

namespace App\Http\Controllers;
use Illuminate\Http\Request;

use Illuminate\Support\Facades\Validator;
use Illuminate\Support\Facades\Auth;
use App\Models\User;

class AuthController extends Controller
{
    /**
     * Create a new AuthController instance.
     *
     * @return void
     */
    public function __construct() {
        $this->middleware('auth:api', ['except' => ['login', 'register']]);
    }

    /**
     * Get a JWT via given credentials.
     *
     * @return \Illuminate\Http\JsonResponse
     */
    public function login(Request $request){
    	$validator = Validator::make($request->all(), [
            'email' => 'required|email',
            'password' => 'required|string|min:6',
        ]);

        if ($validator->fails()) {
            return response()->json($validator->errors(), 422);
        }

        if (! $token = auth()->attempt($validator->validated())) {
            return response()->json(['error' => 'Unauthorized'], 401);
        }

        return $this->createNewToken($token);
    }

    /**
     * Register a User.
     *
     * @return \Illuminate\Http\JsonResponse
     */
    public function register(Request $request) {
        $validator = Validator::make($request->all(), [
            'name' => 'required|string|between:2,100',
            'email' => 'required|string|email|max:100|unique:users',
            'password' => 'required|string|confirmed|min:6',
        ]);

        if($validator->fails()){
            return response()->json($validator->errors()->toJson(), 400);
        }

        $user = User::create(array_merge(
                    $validator->validated(),
                    ['password' => bcrypt($request->password)]
                ));

        return response()->json([
            'message' => 'User successfully registered',
            'user' => $user
        ], 201);
    }


    /**
     * Log the user out (Invalidate the token).
     *
     * @return \Illuminate\Http\JsonResponse
     */
    public function logout() {
        auth()->logout();

        return response()->json(['message' => 'User successfully signed out']);
    }

    /**
     * Refresh a token.
     *
     * @return \Illuminate\Http\JsonResponse
     */
    public function refresh() {
        return $this->createNewToken(auth()->refresh());
    }

    /**
     * Get the authenticated User.
     *
     * @return \Illuminate\Http\JsonResponse
     */
    public function userProfile() {
        return response()->json(auth()->user());
    }

    /**
     * Get the token array structure.
     *
     * @param  string $token
     *
     * @return \Illuminate\Http\JsonResponse
     */
    protected function createNewToken($token){
        return response()->json([
            'access_token' => $token,
            'token_type' => 'bearer',
            'expires_in' => auth()->factory()->getTTL() * 60,
            'user' => auth()->user()
        ]);
    }

}
```

Bom, prefiro explicar todas estas funções pessoalmente, mas se você as lê com calma as entenderá facilmente. 

Agora adicionaremos as rotas no arquivo routes/api.php e substituiremos seu conteúdo por este:

```php
<?php

use Illuminate\Http\Request;
use Illuminate\Support\Facades\Route;
use App\Http\Controllers\AuthController;

/*
|--------------------------------------------------------------------------
| API Routes
|--------------------------------------------------------------------------
|
| Here is where you can register API routes for your application. These
| routes are loaded by the RouteServiceProvider within a group which
| is assigned the "api" middleware group. Enjoy building your API!
|
*/

Route::group([
    'middleware' => 'api',
    'prefix' => 'auth'

], function ($router) {
    Route::post('/login', [AuthController::class, 'login']);
    Route::post('/register', [AuthController::class, 'register']);
    Route::post('/logout', [AuthController::class, 'logout']);
    Route::post('/refresh', [AuthController::class, 'refresh']);
    Route::get('/user-profile', [AuthController::class, 'userProfile']);    
});
```

Agora fecharemos iniciando o serviço interno do Laravel para testes.

```powershell
php artisan serve
```

Dessa forma poderemos utilizar um aplicativo de requisições REST API para testar as funcionalidades de nosso sistema de autenticação. 

| Método | Endpoint               |
| ------ |------------------------|
| POST   | /api/auth/register     |
| POST   | /api/auth/login        |
| GET    | /api/auth/user-profile |
| POST   | /api/auth/refresh      |
| POST   | /api/auth/logout       |

