1. From [Laravel](https://laravel.com/docs/5.5#installing-laravel) in Via Composer Create-Project  
copy `composer create-project --prefer-dist laravel/laravel blog "5.5.*"`
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
