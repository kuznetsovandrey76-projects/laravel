`php artisan make:migration create_task2_table --create=task2`  
в `database/migrations/{созданная миграция}` добавляем `$table->text('body');`    
`php artisan migrate`  

routes/web.php  
```
Route::get('/task2', 'Front\Task2@index');
Route::get('/delete/{title}', 'Front\Task2@delete');
Route::post('/create', 'Front\Task2@insert');
Route::post('/update', 'Front\Task2@update');
```
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
        $task2 = DB::table('task2')->get()->reverse();
        return view('front.task2.index', compact('task2'));
    }

    public function delete(Request $request)
    {
        $id = $request -> route('title');
        DB::delete('delete from task2 where id = ?',[$id]);
        return redirect('/task2')->with('delete','Запись удалена');
    }

    public function insert(Request $request)
    {
        $body = $request -> input('title');
        $data = array('body' => $body);
        DB::table('task2')->insert($data);
        return redirect('/task2')->with('insert','Запись добавлена');
    }

    public function update(Request $request)
    {
        $id = $request -> input('id');
        $body = $request -> input('body');
        DB::table('task2')->where('id',$id)->update(array(
            'body' => $body,
        ));
        return redirect('/task2')->with('update','Запись обновлена');
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
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
    <style>
        .alert_delete {
            color: red;
        }
        .alert_insert {
            color: green;
        }
        a {
            margin: 0 5px;  
        }
        .id {
            display: none;
        }
    </style>
</head>
<body>

    <form action="/create" method="post">
        @csrf
        <input type="text" name="title" placeholder="Input your text">
        <input type="submit" value="Add">
    </form>

    <ul>
        @foreach ($task2 as $task)
            <li><span class="id">{{ $task->id }}</span><span  class="body">{{ $task->body }}</span><a href="delete/{{$task->id}}">X</a><span class="edit">edit</span></li>
            
        @endforeach
    </ul>

    <div id="edit">
        <form action="/update" method="post">
            @csrf
            <input id="edit_id" type="hidden" name="id">
            <input id="edit_body" type="text" name="body">
            <input type="submit" value="Update">
        </form>
    </div>    

    @if (session('delete'))
    <div id="delete" class="alert alert_delete">
        {{ session('delete') }}
    </div>
    @endif

    @if (session('insert'))
    <div id="insert" class="alert alert_insert">
        {{ session('insert') }}
    </div>
    @endif

    <script>
    $( document ).ready(function() {
        setTimeout(function() {
            $('#delete').hide();
            $('#insert').hide();
        }, 2000);

        $('.edit').click(function() {
            var id = $(this).parent().children('.id').text();
            var body = $(this).parent().children('.body').text();
            console.log(id, body);
            // $('#edit').empty();
            // $('<div>', {
                // text: id + ' ' + body
            // }).appendTo('#edit');
            $('#edit_id').val(id);
            $('#edit_body').val(body);
        });
    });

    </script>
</body>
</html>
```

