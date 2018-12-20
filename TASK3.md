`php artisan make:migration create_task2_table --create=task2`

 $table->text('body');

`php artisan migrate`

Route::get('/task2', 'Front\Task2@index');


Task2.php
```php
<?php
namespace App\Http\Controllers\Front;
use App\Http\Controllers\Controller;
use Illuminate\Http\Request;
use DB;

class Task2 extends Controller
{
    public function index()
    {
        $task2 = DB::table('task2')->get();
        return view('front.task2.index', compact('task2'));
    }
}
```
index.blade.php
```php
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <ul>
        @foreach ($task2 as $task)
            <li>
                <!-- <a href="task/{{$task->id}}"> -->
                    {{ $task->body }}
                <!-- </a> -->
            </li>
        @endforeach
    </ul>
</body>
</html>
```
