скачиваем [composer](https://getcomposer.org/download/)  
во время установки указываем расположение `php.exe` как `OSPanel\modules\php\PHP-7.*` - OpenServer     

1. From [Laravel](https://laravel.com/docs/5.5#installing-laravel) in Via Composer Create-Project  
copy `composer create-project --prefer-dist laravel/laravel blog "5.5.*"`

или в папке `domains` - `composer create-project --prefer-dist laravel/laravel [name-your-project]`  
изменяем поля работы с БД в `.env`  
`php artisan make:auth`  
`composer require laravel/socialite`  
`composer require socialiteproviders/vkontakte`  

2. Запустить OpenServer, открыть консоль OpenServera, перейти в `cd domains` и вставить  
3. добавляем домен в OpenServer `\blog\public`
4. устанавливаем [eloquent-sluggable](https://github.com/cviebrock/eloquent-sluggable) в `\blog`  
`composer require cviebrock/eloquent-sluggable:^4.3`
5. В `config\app.php` `'providers' => [`  
добавляем `Cviebrock\EloquentSluggable\ServiceProvider::class,`
6. Устанавливаем из [laravelcollective](https://laravelcollective.com/docs/master/html) в `\blog`   
`composer require "laravelcollective/html":"^5.4.0"`
7. в `public\index.php` после `<?php` добавляем
``` php
phpinfo();
die;
```
запускаем сайт `ctrl+a ctrl+c`  
8. устанавливаем [xdebug](https://xdebug.org/wizard.php) вставляем текст страницы  
можно `Дополнительно->Конфигурации->PHP7.1` вставляем в самый конец  
9. убираем из `public\index.php`


`config/services.php`
```php
'github' => [
  'client_id' => env('GITHUB_CLIENT_ID'),
  'client_secret' => env('GITHUB_CLIENT_SECRET'),
  'redirect' => env('GITHUB_CALLBACK_URL'),
],
```
`.env`
```
GITHUB_CLIENT_ID=
GITHUB_CLIENT_SECRET=
GITHUB_CALLBACK_URL=
```

`resources/vies/auth/login.blade.php`
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
