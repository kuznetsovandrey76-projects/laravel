_commands_
```
php artisan make:migration create_task_table --create=tasks
php artisan migrate
php artisan migrate:refresh
```
.env
```
DB_HOST=localhost
DB_DATABASE=samovar_engine
DB_USERNAME=root
DB_PASSWORD=
```
routes/web.php
```php
<?php
Route::get('/task', 'Front\Task@index');
Route::get('/task/{title}', 'Front\Task@task');
```
app/Http/Controllers/Front/Task.php
```php
<?php
namespace App\Http\Controllers\Front;
use App\Http\Controllers\Controller;
use Illuminate\Http\Request;
use DB;

class Task extends Controller
{
    public function index()
    {
        $tasks = DB::table('tasks')->get();
        return view('front.task.index', compact('tasks'));
    }

    public function task(Request $request)
    {
        $id = $request -> route('title');
        $task = DB::table('tasks')->find($id);
        // dd($task);
        return view('front.task.show', compact('task'));
    }
}
```
database/migrations/2018...
```php
$table->increments('id');
$table->text('body');
$table->timestamps();
```
resources/views/front/task/index.blade.php
```php
<ul>
    @foreach ($tasks as $task)
        <li>
            <a href="task/{{$task->id}}">
                {{ $task->body }}
            </a>
        </li>
    @endforeach
</ul>
```
resources/views/front/task/show.blade.php
```php
<h1>{{ $task->body }}</h1>
<a href="/task">tasks</a>
```
