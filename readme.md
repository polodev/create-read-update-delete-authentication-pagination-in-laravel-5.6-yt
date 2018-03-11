# create read update delete and authentication in laravel   

basic mvc structure 

~~~bash
route 
 ||
controller <====> model 
 ||
view
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

factories and seeds only change when we work with dummy/test data

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

we can make all 4 file using single  command by adding a(all) flag    

~~~php
php artisan make:model YourModel -a
~~~







