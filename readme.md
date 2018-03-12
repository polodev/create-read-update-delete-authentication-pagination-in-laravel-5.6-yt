# create read update delete, authentication and pagination in laravel 5.6               

## basic mvc structure                    

~~~bash
route 
 ||
controller <====> model 
 ||
view
~~~

## software requirements 
php, mysql, <a href="https://getcomposer.org/">composer</a> need to be installed in your mac, pc or linux system before creating project in laravel   

## installing laravel using composer 

~~~php
composer create-project laravel/laravel YourProjectName
~~~

## file we usually make in laravel 

~~~php
app/Http/Controllers/Yourcontroller.php
app/yourmodel.php 
resoures/views/Yourview.php
database/factories/yourfactory.php
database/migrations/yourmigrationfile.php
~~~

## file we usually change in laravel 

~~~php
routes/web.php
database/seeds/databaseSeeder.php
~~~

factories and seeds only require when we work with dummy/test data

## command for making model, controller, factory and migration file 

~~~php
php artisan make:model YourModel
php artisan make:controller YourController
php artisan make:migration create_your_table_name
php artisan make:factory YourModel -m "app\yourmodelname"
~~~

here `php artisan make:controller YourController` will give me basic controller if we add resource flag after this command it will make a resource controller 

~~~php
php artisan make:controller YourController -r
~~~

we can make all 4 file using single  command by adding a flag    

~~~php
php artisan make:model YourModel -a
~~~

## connecting with dabase inside .env file . Whenever we change .env file we have to restart our artisan server 

~~~php
DB_DATABASE=blog
DB_USERNAME=root
DB_PASSWORD
~~~

----------------------------------------------------------------------------------

# Remember to always import class name on top of the file when necessary

----------------------------------------------------------------------------------

## defining default string maximum 191 chracter length inside AppServiceProvider boot method

~~~php
Schema::defaultStringLength(191);
~~~


## writing database table structure inside `create_posts_table` migration file

~~~php
$table->string('title');
$table->text('content');
~~~

## migrating table to database 

~~~php
php artisan migrate 
~~~

## writing dummy data inside `PostFactory.php` using faker instance 

~~~php
'title' => $faker->sentence,
'content' => $faker->paragraph(6)
~~~

## calling factory inside `DatabaseSeeder.php` file

~~~php
# inside run method
factory(Post::class, 30)->create();
~~~

Here I generating 30 dummy post 

## seeding database with dummy data   

~~~php
php artisan db:seed
~~~

## resource route in laravel 

~~~php
Route::resource('posts', 'PostController');
~~~

~~~bash
+--------+-----------+-------------------+---------------+---------------------------------------------+--------------+
| Domain | Method    | URI               | Name          | Action                                      | Middleware   |
+--------+-----------+-------------------+---------------+---------------------------------------------+--------------+
|        | GET|HEAD  | posts             | posts.index   | App\Http\Controllers\PostController@index   | web          |
|        | POST      | posts             | posts.store   | App\Http\Controllers\PostController@store   | web          |
|        | GET|HEAD  | posts/create      | posts.create  | App\Http\Controllers\PostController@create  | web          |
|        | GET|HEAD  | posts/{post}      | posts.show    | App\Http\Controllers\PostController@show    | web          |
|        | PUT|PATCH | posts/{post}      | posts.update  | App\Http\Controllers\PostController@update  | web          |
|        | DELETE    | posts/{post}      | posts.destroy | App\Http\Controllers\PostController@destroy | web          |
|        | GET|HEAD  | posts/{post}/edit | posts.edit    | App\Http\Controllers\PostController@edit    | web          |
+--------+-----------+-------------------+---------------+---------------------------------------------+--------------+
~~~

## basic get route in laravel 

~~~php
Route::get('/', 'YourController@method');
~~~

## showing a view in side a method 

~~~php
 public function index () {
  return view ('posts.index');
  # or
  return view ('posts/index');
 }
 ~~~ 

 ## passing variable in a view 

 ~~~php
  $posts = Post::all();

  return view ('posts.index', ['posts' => $posts]);
  # or
  return view ('posts.index', compact('posts'));
 ~~~

 ## validating in laravel 

~~~php
 
$this->validate($request, [
    'title' => 'required|min:3',
    'content' => 'required|min:10'
]);

~~~

## showing validation error inside view file 

~~~php
@if($errors->all())
  <div class="alert alert-danger">
    @foreach($errors->all() as $error)
    <li>{{$error}}</li>
    @endforeach
  </div>
@endif
~~~

## csrf(cross site request forgery) token and method function inside form 

~~~php
# cross site request forgery   
@csrf 
# for update
@method('put')
# for delete
@method('delete')
~~~

## yielding in master view for content section

~~~php
# inside master.blade.php
@yield('content')
~~~

## extends master in view file and write content inside content section

~~~php
# inside index.balde.php
@extends('master')
@section('content')

@endsection
~~~

## iterating variable using blade directives

~~~php
@foreach($posts as $post)
{{$post->title}}
@endforeach
~~~

## pagination in laravel 

~~~php
## inside controller 
$posts = Post::orderBy('id', 'desc')->paginate(10);

## inside view 
$posts->links()

~~~


 ## redirect in laravel 

 ~~~php
 redirect('/posts');
 redirect(route('login'));
 redirect()->back();
 ~~~

## some model (eloquent) query we are use today

~~~php
## for fetching posts from db 
$posts = Post::all();
$posts = Post::orderBy('id', 'desc')->paginate(10);

## for creating new record 

Post::create([
  'title' => $request->title,
  'content' => $request->content,
]);

## for deleting 

$post->delete();

~~~


## built in authentication in laravel 

~~~php
php artisan make:auth   
~~~


## adding middleware in controller inside  construct function

~~~php
public function __construct () {
    $this->middleware('auth')->except(['index', 'show']);
}
~~~

## adding login logout inside master.blade.php file 

~~~php
@auth
<form class="d-inline-block float-right" action="{{route('logout')}}" method="post">
  @csrf
  <button class="btn btn-secondary">{{auth()->user()->name}} | Logout</button>
</form>
@else 
  <a href="{{route('login')}}" class="btn btn-secondary">Login</a>
@endauth
~~~

## access authenticate user data 

~~~php
auth()->user()->name
~~~

##  how to redirect to certain routes after login or register   

~~~php
# RegisterController.php 
# LoginController.php

protected $redirectTo = '/posts';
~~~

## how to run other people's laravel project in your pc   

~~~php
# installing vendor 
composer install

# clearing old config
php artisan config:clear
php artisan cache:clear

# making .env file 
copy .env.example file to .env file and changed database credentials    

# generating unique key for laravel app
php artisan key:generate

# migrate to database 
php artisan migrate

# seed with dummy data 
php artisan db:seed

# run local development server 
php artisan serve

~~~


## further reading    
<a href="https://laravel.com/docs/5.6">https://laravel.com/docs/5.6</a>         

## watch laracast jeffrey way's tutorial. He is the best web trainer in the planet. You will agree eventually after watching his tutorial. 
<a href="https://laracasts.com/">https://laracasts.com/</a>               


## Thank You

My name is Shibu deb polo      
Thanks for watching    
Take care      





