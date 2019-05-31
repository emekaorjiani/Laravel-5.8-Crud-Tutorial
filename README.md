# Laravel-5.8-Crud-Tutorial
This is simple Laravel 5.8 tutorial for beginner who want to learn basic Crud Mysql Database Operation in Laravel 5.8 framework with extra feature like file upload.


Step by Step CRUD Operation in Laravel 5.8 with File Upload
 Webslesson     22:41     crud laravel, crud laravel 5.8, crud laravel example, crud upload file laravel, crud with file upload, laravel 5.8, laravel 5.8 crud, laravel 5.8 crud example, laravel crud, laravel crud file upload     No comments   
Part 1 - Introduction




Part 2 - Fetch and Display Data




Part 3 - Insert or Add Data with File Upload



Part 4 - Fetch Single Data from Database



Part 5 - Edit or Update Existing MySQL Data



Part 6 - Delete data from Database




If you want to learn How can we do CRUD(Create, Read, Update, Delete) operation with upload file in new version of Laravel 5.8. So, from this post, you can find step by step process of doing CRUD with Mysql database from Laravel 5.8 application. For do CRUD in Laravel 5.8, first you have to upgrade your Laravel framework. For upgrade Laravel you have to go to this Laravel Upgrade Link. Contributors of Laravel has continue improvement in Laravel every version. So, in Laravel 5.8 version you can find something new as from Laravel 5.7. There are following new feature introduced in Laravel 5.8.

has-one-through Eloquent relationships.
Improved email validation.
convention-based automatic registration of authorization policies.
DynamoDB cache and session drivers.
Improved scheduler timezone configuration.
Support for assigning multiple authentication guards to broadcast channels.
PSR-16 cache driver compliance, improvements to the artisan serve command.
PHPUnit 8.0 support.
Carbon 2.0 support.
Pheanstalk 4.0 support, and a variety of other bug fixes and usability improvements.

If you want to get detailed information on Laravel 5.8, then you have go to this Laravel 5.8 relase Link.

Laravel 5.8 Server Requirements

If you want to use Laravel 5.8 for your web development, first you have to check what is minimum server requirement for using Laravel 5.8. Below you can find Laravel 5.8 server requirement.

PHP >= 7.1.3
OpenSSL PHP Extension
PDO PHP Extension
Mbstring PHP Extension
Tokenizer PHP Extension
XML PHP Extension
Ctype PHP Extension
JSON PHP Extension
BCMath PHP Extension

Suppose you have check all above server requirement, and you have all above requirement, then you can start using Laravel 5.8 framework version. So, here first we will make simple CRUD Application in Laravel 5.8, for this you have to following steps.


Step by Step CRUD Operation in Laravel 5.8 with File Upload


Step 1 - Download Laravel 5.8

In first step you have to download Laravel 5.8 version. For this you have to go to command prompt, in which first you have go to your folder path in which you want to download Laravel 5.8. After this you have to run "composer" command, because all Laravel depository handle by composer. After run "composer" command. You have to run following command.


composer create-project laravel/laravel=5.8 crud --prefer-dist


Above command will make crud folder and under this folder it will download Laravel 5.8.

Step 2 - Mysql Database connection Laravel 5.8

Once you have download Laravel 5.8, so now first we want to make Mysql database connection from Laravel 5.8. You can make Mysql database connection from two way.

In first way you have to find .env file in your Laravel 5.8 folder. Open that file and under you have to define your mysql database configuration like below.


DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=testing
DB_USERNAME=root
DB_PASSWORD=


In second way, you have to open config/database.php file, and under this file you can also define your mysql database configuration.


'mysql' => [
            'driver' => 'mysql',
            'host' => env('DB_HOST', '127.0.0.1'),
            'port' => env('DB_PORT', '3306'),
            'database' => env('DB_DATABASE', 'testing'),
            'username' => env('DB_USERNAME', 'root'),
            'password' => env('DB_PASSWORD', ''),
            'unix_socket' => env('DB_SOCKET', ''),
            'charset' => 'utf8mb4',
            'collation' => 'utf8mb4_unicode_ci',
            'prefix' => '',
            'prefix_indexes' => true,
            'strict' => true,
            'engine' => null,
            'options' => array_filter([
                PDO::MYSQL_ATTR_SSL_CA => env('MYSQL_ATTR_SSL_CA'),
            ]),
        ],


After defining above details you can make Mysql database connection from your Laravel 5.8 application.

Step 3 - Migrate Table from Laravel 5.8 to Mysql Database

In Laravel framework, you have facility from make table in Mysql database from your Laravel application. For this first you have to create migration file in your Laravel folder. For this you have to write following command in your command prompt.


php artisan make:migration create_crud_table --create=crud


This command will create migration file in database/migrations folder. In this file we have to define table column which we want to create in table. Below you can find migration file in which we have define table column.


<?php

use Illuminate\Support\Facades\Schema;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class CreateCrudTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('cruds', function (Blueprint $table) {
            $table->bigIncrements('id');
            $table->string('first_name');
            $table->string('last_name');
            $table->string('image');
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('cruds');
    }
}


Now we want to migrate this table definition from this Laravel application to mysql database. For this we have write following command in command prompt. This command will make crud table in mysql database for perform CRUD operation from Laravel 5.8 application.


php artisan migrate


Step 4 - Create Model file in Laravel 5.8

In this we will seen how can we make model file in Laravel 5.8. This class file mainly used for do database related operation in controller class. For create model files we have to write following command in command prompt.


php artisan make:model Crud -m


This command will make Crud.php model file in app folder. In this file we have to define table column name which you can see below source code of Crud.php file.


<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Crud extends Model
{
    protected $fillable = [
     'first_name', 'last_name', 'image'
    ];
}





Step 5 - Create Controllers in Laravel 5.8

Controllers files mainly used for handle http request in Laravel 5.8 application. Here we want to create Laravel 5.8 crud controller. For this we have to go to command prompt and under this we have write following command.


php artisan make:controller CrudsController --resource


This command will make CrudsController.php file in app/Http/Controllers folder. Once you have open this file, then you can find all predefine method for do CRUD operation in this controller file. We have to just add code for do particular operation. Below you can find CRUD controller file code below.


<?php

namespace App\Http\Controllers;
use App\Crud;
use Illuminate\Http\Request;

class CrudsController extends Controller
{
    /**
     * Display a listing of the resource.
     *
     * @return \Illuminate\Http\Response
     */
    public function index()
    {
        $data = Crud::latest()->paginate(5);
        return view('index', compact('data'))
                ->with('i', (request()->input('page', 1) - 1) * 5);
    }

    /**
     * Show the form for creating a new resource.
     *
     * @return \Illuminate\Http\Response
     */
    public function create()
    {
        return view('create');
    }

    /**
     * Store a newly created resource in storage.
     *
     * @param  \Illuminate\Http\Request  $request
     * @return \Illuminate\Http\Response
     */
    public function store(Request $request)
    {
        $request->validate([
            'first_name'    =>  'required',
            'last_name'     =>  'required',
            'image'         =>  'required|image|max:2048'
        ]);

        $image = $request->file('image');

        $new_name = rand() . '.' . $image->getClientOriginalExtension();
        $image->move(public_path('images'), $new_name);
        $form_data = array(
            'first_name'       =>   $request->first_name,
            'last_name'        =>   $request->last_name,
            'image'            =>   $new_name
        );

        Crud::create($form_data);

        return redirect('crud')->with('success', 'Data Added successfully.');
    }

    /**
     * Display the specified resource.
     *
     * @param  int  $id
     * @return \Illuminate\Http\Response
     */
    public function show($id)
    {
        $data = Crud::findOrFail($id);
        return view('view', compact('data'));
    }

    /**
     * Show the form for editing the specified resource.
     *
     * @param  int  $id
     * @return \Illuminate\Http\Response
     */
    public function edit($id)
    {
        $data = Crud::findOrFail($id);
        return view('edit', compact('data'));
    }

    /**
     * Update the specified resource in storage.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  int  $id
     * @return \Illuminate\Http\Response
     */
    public function update(Request $request, $id)
    {
        $image_name = $request->hidden_image;
        $image = $request->file('image');
        if($image != '')
        {
            $request->validate([
                'first_name'    =>  'required',
                'last_name'     =>  'required',
                'image'         =>  'image|max:2048'
            ]);

            $image_name = rand() . '.' . $image->getClientOriginalExtension();
            $image->move(public_path('images'), $image_name);
        }
        else
        {
            $request->validate([
                'first_name'    =>  'required',
                'last_name'     =>  'required'
            ]);
        }

        $form_data = array(
            'first_name'       =>   $request->first_name,
            'last_name'        =>   $request->last_name,
            'image'            =>   $image_name
        );
  
        Crud::whereId($id)->update($form_data);

        return redirect('crud')->with('success', 'Data is successfully updated');
    }

    /**
     * Remove the specified resource from storage.
     *
     * @param  int  $id
     * @return \Illuminate\Http\Response
     */
    public function destroy($id)
    {
        $data = Crud::findOrFail($id);
        $data->delete();

        return redirect('crud')->with('success', 'Data is successfully deleted');
    }
}


index() - In Laravel Crud controller index() method is a root method of crud controller, so when in browser we have enter base url with controller name, then it will called this index() method. Under this method we will write code for display data from mysql database. In this code it will first fetch data from crud table and store under $data variable, after this we want to make paginate link by using paginate() method in Laravel 5.8. For send data to view file, for this we have use view() method for send data to view file. Source code you can find under index() method.

create() - This method has been used for load create.blade.php file. In this file user can find form for insert new records and filling data insert data request will be send to store() method of CrudsController.php controller class.

store() - This method has received add or insert new records request received from create() method. In this method, it will perform two operation. One is for upload image file by using move() method and second is for insert records into mysql table by using model class. Before upload of image, it will rename image name. After successfully insert or add of data in mysql table from this Laravel 5.8 application, page will redirect to index() method with success message.

show() - This method in Crud controller has been used for fetch single data details based on on value of $id argument. For this it has been used findOrFail() method. After fetch details it will send to view.blade.php file.

edit() - This method main function is fetch single data from Mysql database and load into edit or update form for make required changes.

update() - Update method has received edit or update data request from edit(). This method has done two function like upload of profile image with update or edit mysql data in Laravel 5.8 framework.

delete() - Delete() method mainly used for remove single or multiple data from Mysql Database. This is last operation Crud Operation in Laravel 5.8 Crud tutorial series.


See Also
Import Excel File in Laravel
Ajax jQuery Load More Data in Laravel
Laravel Date Range Search using Ajax jQuery
Live Table Add Edit Delete in Laravel using Ajax jQuery
Simple Way to Sending an Email in Laravel


Step 6 - Set Route in Laravel 5.8

This is very useful step in Laravel 5.8, because here we want to set route of all CrudsController class method. For this we have to open to routes/web.php file. In this file we have to write following code for set route of all method.


Route::resource('crud','CrudsController');


Step 7 - Set Data in View File in Laravel 5.8

This is the last step in Laravel 5.8 Crud application, and in this step we have to set data in view file which has been store under resources/views folder, because this view file has received data from controller method, so here we have to set data in view file. Below you can file all view file which has been used in Crud application, and you can also find how data has been set and how to make form in Laravel 5.8 view file.

resources/views/parent.blade.php 

<html>
 <head>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Laravel 5.8 Crud Tutorial</title>
  <meta content='width=device-width, initial-scale=1, maximum-scale=1' name='viewport'/>
  <script src="https://ajax.googleapis.com/ajax/libs/jquery/2.1.3/jquery.min.js"></script>
  <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/js/bootstrap.min.js"></script>
  <link href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css" rel="stylesheet" />
 </head>
 <body>
  <div class="container">    
     <br />
     <h3 align="center">Laravel 5.8 Crud Tutorial</h3>
     <br />
     @yield('main')
    </div>
 </body>
</html>


resources/views/index.blade.php 

@extends('parent')

@section('main')

<table class="table table-bordered table-striped">
 <tr>
  <th width="10%">Image</th>
  <th width="35%">First Name</th>
  <th width="35%">Last Name</th>
  <th width="30%">Action</th>
 </tr>
 @foreach($data as $row)
  <tr>
   <td><img src="{{ URL::to('/') }}/images/{{ $row->image }}" class="img-thumbnail" width="75" /></td>
   <td>{{ $row->first_name }}</td>
   <td>{{ $row->last_name }}</td>
   <td>
    
   </td>
  </tr>
 @endforeach
</table>
{!! $data->links() !!}
@endsection


resources/views/create.blade.php 

@extends('parent')

@section('main')
@if($errors->any())
<div class="alert alert-danger">
 <ul>
  @foreach($errors->all() as $error)
  <li>{{ $error }}</li>
  @endforeach
 </ul>
</div>
@endif
<div align="right">
 <a href="{{ route('crud.index') }}" class="btn btn-default">Back</a>
</div>

<form method="post" action="{{ route('crud.store') }}" enctype="multipart/form-data">

 @csrf
 <div class="form-group">
  <label class="col-md-4 text-right">Enter First Name</label>
  <div class="col-md-8">
   <input type="text" name="first_name" class="form-control input-lg" />
  </div>
 </div>
 <br />
 <br />
 <br />
 <div class="form-group">
  <label class="col-md-4 text-right">Enter Last Name</label>
  <div class="col-md-8">
   <input type="text" name="last_name" class="form-control input-lg" />
  </div>
 </div>
 <br />
 <br />
 <br />
 <div class="form-group">
  <label class="col-md-4 text-right">Select Profile Image</label>
  <div class="col-md-8">
   <input type="file" name="image" />
  </div>
 </div>
 <br /><br /><br />
 <div class="form-group text-center">
  <input type="submit" name="add" class="btn btn-primary input-lg" value="Add" />
 </div>

</form>

@endsection


resources/views/view.blade.php

@extends('parent')

@section('main')

<div class="jumbotron text-center">
 <div align="right">
  <a href="{{ route('crud.index') }}" class="btn btn-default">Back</a>
 </div>
 <br />
 <img src="{{ URL::to('/') }}/images/{{ $data->image }}" class="img-thumbnail" />
 <h3>First Name - {{ $data->first_name }} </h3>
 <h3>Last Name - {{ $data->last_name }}</h3>
</div>
@endsection


resources/views/edit.blade.php

@extends('parent')

@section('main')
            
            @if ($errors->any())
                <div class="alert alert-danger">
                    <ul>
                        @foreach ($errors->all() as $error)
                            <li>{{ $error }}</li>
                        @endforeach
                    </ul>
                </div>
            @endif
            <div align="right">
                <a href="{{ route('crud.index') }}" class="btn btn-default">Back</a>
            </div>
            <br />
     <form method="post" action="{{ route('crud.update', $data->id) }}" enctype="multipart/form-data">
                @csrf
                @method('PATCH')
      <div class="form-group">
       <label class="col-md-4 text-right">Enter First Name</label>
       <div class="col-md-8">
        <input type="text" name="first_name" value="{{ $data->first_name }}" class="form-control input-lg" />
       </div>
      </div>
      <br />
      <br />
      <br />
      <div class="form-group">
       <label class="col-md-4 text-right">Enter Last Name</label>
       <div class="col-md-8">
        <input type="text" name="last_name" value="{{ $data->last_name }}" class="form-control input-lg" />
       </div>
      </div>
      <br />
      <br />
      <br />
      <div class="form-group">
       <label class="col-md-4 text-right">Select Profile Image</label>
       <div class="col-md-8">
        <input type="file" name="image" />
              <img src="{{ URL::to('/') }}/images/{{ $data->image }}" class="img-thumbnail" width="100" />
                        <input type="hidden" name="hidden_image" value="{{ $data->image }}" />
       </div>
      </div>
      <br /><br /><br />
      <div class="form-group text-center">
       <input type="submit" name="edit" class="btn btn-primary input-lg" value="Edit" />
      </div>
     </form>

@endsection


Step 8 - Run Laravel 5.8 Application

Lastly, we want to run Laravel 5.8 Crud application, for this we have to go to command prompt, and write following command.


php artisan serve


Above command will start Laravel 5.8 application. For run Larave 5.8 crud file, you have to write this url http://127.0.0.1:8000/crud in your browser for test code is working or not.

Here will make simple CRUD application with uploading file in Laravel 5.8 version. 


Download Source Code
