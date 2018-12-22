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
    <title>Task 2</title>
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
    <style>
        body {
            margin: 0;
        }
        .form {
            background: lightgray;
            display: flex;
            justify-content: center;
            padding: 10px 0;
            margin-bottom: 10px;         
        }
        .form__input {
            padding: 3px;
        }
        .form__submit {
            background: lightgreen;
            border: none;
            padding: 5px;            
        }
        .table {
            margin: 0 auto;
            text-align: center;
            margin-bottom: 10px; 
        }
        .alert {
            text-align: center;
        }
        .alert_delete {
            color: red;
        }
        .alert_insert {
            color: green;
        }
        .alert_update {
            color: brown;
        }
        .edit-block {
            display: flex;
            justify-content: center;
        }
        #popup_close {
            margin: 0 5px;
            cursor: pointer;
        }
        .edit {
            cursor: pointer;
        }
        a {
            margin: 0 5px;  
        }
        .id {
            display: none;
        }
        .popup{
            width:100%;
            min-height:100%;
            background-color: rgba(0,0,0,0.5);
            overflow:hidden;
            position:fixed;
            top:0px;
        }
        .popup .popup__container{
            margin:40px auto 0px auto;
            display: flex;
            width: 300px;
            padding:10px;
            background-color: #c5c5c5;
            border-radius:5px;
            box-shadow: 0px 0px 10px #000;
        }
        .popup__container {
            display: flex;
            justify-content: center;
            width: auto;
        }
    </style>
</head>
<body>
    <div class="form form_main">            
        <form action="/create" method="post">
            @csrf
            <input class="form__input" type="text" name="title" placeholder="Введите сообщение">
            <input class="form__submit" type="submit" value="Добавить">
        </form>
    </div>
    <table class="table">
        <thead>
            <tr>
                <th class="id">Name</th>
                <!-- <th>E-mail</th> -->
                <th>Message</th>
                <th>Update</th>
                <th>Delete</th>
                <!-- <th>Add product</th> -->
                <!-- <th>View product</th> -->
            </tr>
        </thead>
        <tbody>
        @foreach ($task2 as $task)
            <tr>                    
                <td class="id">{{ $task->id }}</td>
                <!-- <td>E-mail</td> -->
                <td class="body">{{ $task->body }}</td>
                <td class="edit">edit</td>
                <td><a href="delete/{{$task->id}}">X</a></td>
            </tr>
        @endforeach
        </tbody>
    </table>


    <div class="popup" id="popup_">
    <div class="popup__container" id="edit">
        <form action="/update" method="post">
            @csrf
            <input id="edit_id" type="hidden" name="id">
            <input id="edit_body" type="text" name="body">
            <input id="edit_update" type="submit" value="Update">
        </form>
        <span id="popup_close">x</span>
    </div>    
    </div>

    @if (session('update'))
    <div id="update" class="alert alert_update">
        {{ session('update') }}
    </div>
    @endif

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
        $('#popup_').hide();
        $('#edit').hide();

        setTimeout(function() {
            $('#delete').hide();
            $('#insert').hide();
            $('#update').hide();
        }, 1000);

        $('.edit').click(function() {
            var id = $(this).parent().children('.id').text();
            var body = $(this).parent().children('.body').text();
            console.log(id, body);
            $('#edit').show();
            // $('#edit').empty();
            // $('<div>', {
                // text: id + ' ' + body
            // }).appendTo('#edit');
            $('#edit_id').val(id);
            $('#edit_body').val(body);
        });

        $('#edit_update').click(function() {
            $('#edit').hide();
        });

        $('.edit').click(function() {
            $('#popup_').show();
        });

        $('#popup_close').click(function() {
            $('#popup_').hide();
        });
    });
    </script>
</body>
</html>
```

