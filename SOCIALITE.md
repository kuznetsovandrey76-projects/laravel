в GitHub `Settings -> Developer settings -> New OAuth App`  
`Authorization callback URL` - `http://[name-your-project]/login/github/callback`  

скачиваем [composer](https://getcomposer.org/download/)  

во время установки указываем расположение `php.exe` как `OSPanel\modules\php\PHP-7.*` - OpenServer     

в папке `domains` - `composer create-project --prefer-dist laravel/laravel [name-your-project]`  

изменяем поля работы с БД в `.env`  

`php artisan make:auth`  

`composer require laravel/socialite`  

`composer require socialiteproviders/vkontakte`  

добавляем домен в OpenServer `\[name-your-project]\public`

в `routes/web.php`
```php
Route::get('login/github', 'Auth\LoginController@redirectToProvider');
Route::get('login/github/callback', 'Auth\LoginController@handleProviderCallback');
```

в `config/services.php`
Добавляем в 
```php
'providers' => [
```
`\SocialiteProviders\Manager\ServiceProvider::class,`

в `config/services.php`
```php
'github' => [
  'client_id' => env('GITHUB_CLIENT_ID'),
  'client_secret' => env('GITHUB_CLIENT_SECRET'),
  'redirect' => env('GITHUB_CALLBACK_URL'),
],
```

в `.env`
```
GITHUB_CLIENT_ID=
GITHUB_CLIENT_SECRET=
GITHUB_CALLBACK_URL=
```

в `resources/vies/auth/login.blade.php`
```php
    </form>
    <a href="{{ url('login/github') }}" class="btn btn-primary">Login GitHub</a>
</div>
```

edit `config/database.php`
```
'charset' => 'utf8mb4',
'collation' => 'utf8mb4_unicode_ci',
```
to
```
'charset' => 'utf8',
'collation' => 'utf8_unicode_ci',
```

в `app/Http/Auth/LoginController.php`
```php
<?php

namespace App\Http\Controllers\Auth;

use App\Http\Controllers\Controller;
use Illuminate\Foundation\Auth\AuthenticatesUsers;

use Socialite;
use App\User;

use Auth;

class LoginController extends Controller
{
    /*
    |--------------------------------------------------------------------------
    | Login Controller
    |--------------------------------------------------------------------------
    |
    | This controller handles authenticating users for the application and
    | redirecting them to your home screen. The controller uses a trait
    | to conveniently provide its functionality to your applications.
    |
    */

    use AuthenticatesUsers;

    /**
     * Where to redirect users after login.
     *
     * @var string
     */
    protected $redirectTo = '/home';

    /**
     * Create a new controller instance.
     *
     * @return void
     */
    public function __construct()
    {
        $this->middleware('guest')->except('logout');
    }

    
    /**
     * Redirect the user to the GitHub authentication page.
     *
     * @return \Illuminate\Http\Response
     */
    public function redirectToProvider()
    {
        return Socialite::driver('github')->redirect();
    }

    /**
     * Obtain the user information from GitHub.
     *
     * @return \Illuminate\Http\Response
     */
    public function handleProviderCallback()
    {
        $github_user = Socialite::driver('github')->user();
        $user = $this->userFindOrCreate($github_user);

        Auth::login($user, true);

        return redirect($this->redirectTo);
        // $user->token;
    }

    public function userFindOrCreate($github_user)
    {
        // print_r($github_user);
        $user = User::where('provider_id', $github_user->id)->first();

        if(!$user) 
        {
            $user = new User;
            $user->name = $github_user->getName();
            $user->email = $github_user->getEmail();
            $user->provider_id = $github_user->getid();
            $user->save();
            
        }

        return $user;
    }
}

```

изменяем миграцию `create_users_table`
```php
Schema::create('users', function (Blueprint $table) {
    $table->increments('id');
    $table->string('name');
    $table->string('email')->unique();
    $table->string('password')->nullable();
    $table->string('provider_id')->nullable();
    $table->rememberToken();
    $table->timestamps();
});
```

`php artisan migrate`
