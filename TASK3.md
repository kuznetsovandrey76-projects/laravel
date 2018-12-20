`php artisan make:migration create_task2_table --create=task2`
database/migrations/{созданная миграция}
добавляем `$table->text('body');`
`php artisan migrate`
routes/web.php
`Route::get('/task2', 'Front\Task2@index');`
app/Http/Front/Task2.php
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
resources/views/front/task2/index.blade.php
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

    <form action="">
        <input type="text" name="title" placeholder="Input your text">
        <input type="submit" value="Add">
    </form>

    <ul>
        @foreach ($task2 as $task)
            <li>{{ $task->body }}<a href="delete/{{$task->id}}">X</a></li>
        @endforeach
    </ul>
</body>
</html>
```
app/Http/Front/Delete.php
```php
<?php

namespace App\Http\Controllers\Front;

use App\Http\Controllers\Controller;
use Illuminate\Http\Request;
use DB;

class Delete extends Controller
{

    public function index(Request $request)
    {
        $id = $request -> route('title');
        DB::delete('delete from task2 where id = ?',[$id]);
        return redirect('/task2')->with('success','Information has been  deleted');
    }
}
```
