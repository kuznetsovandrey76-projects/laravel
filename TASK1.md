1. Route in `routes/web.php`  
``` php
<?php
Route::get('/', 'Front\Index@index');
Route::get('/about', 'Front\Index@about');
Route::get('/page/{title}', 'Front\Index@page');
```
2. Controller in `app/Http/Controllers/Front/index.php`  
``` php
<?php
namespace App\Http\Controllers\Front;

use App\Http\Controllers\Controller;
use Illuminate\Http\Request;

class Index extends Controller
{
    public function index()
    {
        return view('front.home.index');
    }
    public function about()
    {
        return view('front.home.about');
    }
    public function page(Request $request)
    {
        $name = $request -> route('title');
        return view('front.home.page', compact('name'));
    }
}
```
3. View in `resources/views/front/home/page.blade.php`  
``` php
@extends('front.layout.main')

@section('content')
<!-- CONTENT -->
<main class="content">
    <h1>{{ $name }}</h1>
</main>
<!-- END CONTENT -->
@endsection
```
