---
title: Laravel框架记录（一）
tags:
  - laravel
  - php
url: 49.html
id: 49
categories:
  - PHP是最好的语言
date: 2018-05-17 20:33:36
---

Lavavel
-------

Lavavel是PHP的主流轻量框架之一。

#### install and config

1.  php environment
    
    *   PHP >=7.1.3
        
    *   PHP OpenSSL etend
        
    *   PHP PDO
        
    *   PHP Mbstring
        
    *   PHP Tokenizer
        
    *   PHP XML
        
    *   PHP Ctype
        
    *   PHP JSON
        
2.  install Laravel with Composer
    
    install Laravel installer
    
    `composer global require “laravel/installer”`
    
    cd new floder
    
    `laravel new xxx`
    
3.  start server
    
    `php artisan serve`
    

#### 目录结构

*   App目录：包含了应用的核心代码
    
*   Boostrap目录（不是css框架哦！）：包含了少许文件，用于框架的启动和自动载入配置，cache放置了路由和服务缓存文件
    
*   Config目录：包含了应用所有的配置文件
    
*   Database目录：数据库迁移文件级填充文件
    
*   Public目录：包含了应用入口文件和index.php和前端资源文件
    
*   Resources目录：包含了应用视图文件和未编译的原生前端资源文件
    
*   Routes目录
    
    *   api.php——包含路由位于`api`中间件约束之内，支持频率限制功能。
        
    *   web.php——定义路由位于`RouteServiceProvider`所定义的`web`中间件约束中，因而支持Session、CSRF保护以及Cookie加密功能。
        
    *   console.php——文件用于定义所有基于闭包的控制台命令
        
    *   channels.php——文件用于注册应用支持的所有事件广播频道
        
*   Storage目录
    
    包含了编译后的Blade模板、基于文件的Session、文件缓存，以及其他框架生成的文件：
    
    *   app——应用生成的文件
        
    *   framework——框架生成的文件和缓存
        
    *   logs——应用的日志文件
        
*   Test目录
    
    包含自动化测试文件，默认提供了一个开箱即用的PHPUnit实例
    
*   Vendor目录
    
    包含了应用所有通过Composer加载的依赖
    

#### Laravel的生命周期

##### 入口

Laravel所有的请求入口都为`public\index.php`文件，是加载框架其他文件的起点。 index.php文件载入composer生成的自动加载设置，然后从bootstrap/app.php脚本获取Laravel应用实例，Laravel的第一个动作就是创建服务器实例

##### Http/console内核

接下来，请求被发送到Http内核或console内核，这取决于进入应用的请求类型，这两个内核， 是所有请求都要经过的中央处理器。 Http内核继承自`IIlluminate\Foundation\Http\Kernel`类

##### 服务提供者

内核启动过程中最重要的动作之一便是为应用载入服务提供者，应用的所以服务提供者都被配置在`config/app.php`配置文件的providers数组当中。首先，所有提供者的register方法被调用，然后，所有提供者被注册后，boot方法被调用。 服务提供者负责启动框架的所有各种各样的组件，比如数据库、队列、验证器等。

##### 分发请求

一旦应用被启动并且所有的服务提供者被注册，Request将会交由路由器进行分发，路由器将会分发请求到路由或控制器，同时运行所有路由指定的中间件。

#### Artisan命令

Artisan 就相当于 Laravel 为我们独家定制的一个命令行工具，提供了很多实用的命令，可以用来快速生成 Laravel 开发中常用的一些文件并完成相关的配置。 命令 说明 php artisan key:generate 生成 App Key php artisan make:controller 生成控制器 php artisan make:model 生成模型 php artisan make:policy 生成授权策略 php artisan make:seeder 生成 Seeder 文件 php artisan migrate 执行迁移 php artisan migrate:rollback 回滚迁移 php artisan migrate:refresh 重置数据库 php artisan db:seed 填充数据库 php artisan tinker 进入 tinker 环境 php artisan route:list 查看路由列表

#### 控制器

​

#### 数据库和数据库迁移

Laravel支持四种类型的数据库：

*   MySQL
    
*   Postgres
    
*   SQLite
    
*   SQL Server
    

数据库配置文件在config/database.php中

 <?php
​
 return \[
​
 /*
 |--------------------------------------------------------------------------
 | Default Database Connection Name
 |--------------------------------------------------------------------------
 |
 | Here you may specify which of the database connections below you wish
 | to use as your default connection for all database work. Of course
 | you may use many connections at once using the Database library.
 |
 */
​
 'default' => env('DB_CONNECTION', 'mysql'),
​
 /*
 |--------------------------------------------------------------------------
 | Database Connections
 |--------------------------------------------------------------------------
 |
 | Here are each of the database connections setup for your application.
 | Of course, examples of configuring each database platform that is
 | supported by Laravel is shown below to make development simple.
 |
 |
 | All database work in Laravel is done through the PHP PDO facilities
 | so make sure you have the driver for your particular database of
 | choice installed on your machine before you begin development.
 |
 */
​
 'connections' => \[
​
 'sqlite' => \[
 'driver' => 'sqlite',
 'database' => env('DB_DATABASE', database_path('database.sqlite')),
 'prefix' => '',
 \],
​
 'mysql' => \[
 'driver' => 'mysql',
 'host' => env('DB_HOST', '127.0.0.1'),
 'port' => env('DB_PORT', '3306'),
 'database' => env('DB_DATABASE', 'forge'),
 'username' => env('DB_USERNAME', 'forge'),
 'password' => env('DB_PASSWORD', ''),
 'unix_socket' => env('DB_SOCKET', ''),
 'charset' => 'utf8mb4',
 'collation' => 'utf8mb4\_unicode\_ci',
 'prefix' => '',
 'strict' => true,
 'engine' => null,
 
 \],
​
 'pgsql' => \[
 'driver' => 'pgsql',
 'host' => env('DB_HOST', '127.0.0.1'),
 'port' => env('DB_PORT', '5432'),
 'database' => env('DB_DATABASE', 'forge'),
 'username' => env('DB_USERNAME', 'forge'),
 'password' => env('DB_PASSWORD', ''),
 'charset' => 'utf8',
 'prefix' => '',
 'schema' => 'public',
 'sslmode' => 'prefer',
 \],
​
 'sqlsrv' => \[
 'driver' => 'sqlsrv',
 'host' => env('DB_HOST', 'localhost'),
 'port' => env('DB_PORT', '1433'),
 'database' => env('DB_DATABASE', 'forge'),
 'username' => env('DB_USERNAME', 'forge'),
 'password' => env('DB_PASSWORD', ''),
 'charset' => 'utf8',
 'prefix' => '',
 \],
​
 \],
​
 /*
 |--------------------------------------------------------------------------
 | Migration Repository Table
 |--------------------------------------------------------------------------
 |
 | This table keeps track of all the migrations that have already run for
 | your application. Using this information, we can determine which of
 | the migrations on disk haven't actually been run in the database.
 |
 */
​
 'migrations' => 'migrations',
​
 /*
 |--------------------------------------------------------------------------
 | Redis Databases
 |--------------------------------------------------------------------------
 |
 | Redis is an open source, fast, and advanced key-value store that also
 | provides a richer set of commands than a typical key-value systems
 | such as APC or Memcached. Laravel makes it easy to dig right in.
 |
 */
​
 'redis' => \[
​
 'client' => 'predis',
​
 'default' => \[
 'host' => env('REDIS_HOST', '127.0.0.1'),
 'password' => env('REDIS_PASSWORD', null),
 'port' => env('REDIS_PORT', 6379),
 'database' => 0,
 \],
​
 \],
​
 \];
​

数据库用户及密码等敏感信息位于.env文件当中

 DB_CONNECTION=mysql
 DB_HOST=127.0.0.1
 DB_PORT=3306
 DB_DATABASE=lavarel
 DB_USERNAME=???
 DB_PASSWORD=???

###### 运行原生SQL语句

配置好数据库连接后，可以使用`DB`facade来进行查询

 DB::select、update、delete、insert、statement 
 DB::select('select * from users where active = ？',\[1\]); //返回结果集，每一个 
 DB::select('select * from user where active = :id',\['id'=>1\]);
 DB::statement('drop table users');
 DB::transaction(function(){DB:table('user')->update(\['votes' => 1\]);})
 DB::beginTransaction();
 DB::rollBack();
 DB::commit();
 //监听每一条SQL，可以使用listen方法，该方法对查询日志

###### 查询构造器

Laravel的数据库查询构造器提供了方便、流畅的接口，以用来创建及运行数据库查询。可用来执行应用程序中的大部分数据库操作

 DB::table('users')->get();//查询构造器
 DB::table('users')->where('name','John')->first();//返回单个StdClass对象
 DB::table('users')->where('name','John')->value('email');//获取单个值
 DB::table('roles')->pluck('title'); //获取一个包含单个字段值的数组
 DB::table('users')->orderBy('id')->chunk(100,function($users){
 foreach($users as $user){
 
 }
 })//chunk用于处理大量记录时使用，对结果集进行分块

聚合 查询构造器提供了各种聚合方法，例如`count`、`max`、`min`、`avg` 以及`sum`

 DB::table('users')->count();
 DB::table('orders')->max('price');

Selects

 DB::table('users')->select('name','email as user_email')->get();
 DB::table('users')->distinct()->get();//强制查找返回不重复的结果
 DB::raw-创建一个原始表达式

Inserts

 DB::table('users')->insert(
 \['email' => 'john@example.com', 'votes' => 0\],//插入一行
 \['email' => 'john2@example.com', 'votes' => 0\],//插入多行
 );
 DB::table('users')->insertGetId(
 \['email' => 'john@example.com', 'votes' => 0\]
 )

Updates

 DB::table('users')
 ->where('id',1)
 ->update(\['votes' => 1\]);

递增或递减

 DB::table('users')->increment('votes');
 DB::table('users')->increment('votes',5);

Deletes

 DB::table('users')->delete();
 DB::table('users')->where('votes','>',100)->delete();
 DB::table('users')->truncate();//删除所有数据列

悲观锁定

 DB::table('users')->where('votes','>',100)->sharedLock()->get();
 DB::table('users')->where('votes','>',100)->lockForUpdate()->get();

分页 `paginate方法`

 $users = DB::table('users')->paginate(15);//15规定每页显示15条数据
 return view('user.index',\['users'=> $users\]);

简单分页`simplePaginate()`

 $users = DB::table('users')->simplePaginate(15);//不会在页面上显示页码 

基于Eloquent模型分页

 $users = APP\\User::paginate(15);
 $users = User::where('votes','>',100)->paginate(15);
 $users = User::where('votes','>',100)->simplePaginate(15);

###### 数据库迁移

`make:migration` Artisan命令 database/migrations/xxxx\_xx\_xx\_xxxxx\_create\_xxx\_table.php（数据库迁移文件）

 class CreateUsersTable extends Migration
 {
 /**
 \* Run the migrations.
 *
 \* @return void
 */
 public function up()//添加新的数据表、字段或索引
 {
 Schema::create('users', function (Blueprint $table) {
 $table->increments('id');
 $table->string('name');
 $table->string('email')->unique();
 $table->string('password');
 $table->rememberToken();
 $table->timestamps();
 });
 }
​
 /**
 \* Reverse the migrations.
 *
 \* @return void
 */
 public function down()//up的逆操作
 {
 Schema::dropIfExists('users');
 }
 }

运行迁移（php artisan migrate \[--force\]） 回滚迁移（php artisan migrate:rollback \[--step=5\]） 回滚所有迁移（php artisan migrate:reset) 回滚加迁移 (php artisan migrate:refresh)

 public function up()
 {
 Schema::create('articles', function (Blueprint $table) {
 $table->engine = 'InnoDB';
 $table->increments('id');
 $table->string('content',100);
 $table->Integer('num');//Integer方法的第二个参数并不是指定长度,而是是否设置auto increment，所以integer方法无法指定子段长度，默认为11。
 $table->enum('source',\['bank acount','credit','payPal'\]);
 $table->timestamps();//会自动创建updated\_at、created\_at字段
​
 });
 Schema::rename('articles','articless');
 }

Seeds Laravel使用seed类来给数据库填充测试数据，所有的seed类都放在database/seeds目录下。

 class DatabaseSeeder extends Seeder
 {
 /**
 \* Seed the application's database.
 *
 \* @return void
 */
 public function run()
 {
 // $this->call(UsersTableSeeder::class);
 DB::table('articles')->insert(
 \['content'=>str_random(80),
 'num' => random_int(5, 100),
 'source'=>'bank acount',\]
 }
 }

###### Redis

###### \# # # # # # # # # # # # # # #

##### Eloquent

Laravel的Eloquent ORM提供了漂亮、简洁的ActiveRecord实现来和数据库做交互。每个数据表对应一个模型 定义模型 php artisan make:model Article \[-m(附加数据库迁移文件)\] 模型的使用

 <?php
 use App\\Article;
 $article = App\\Article::all();//返回模型数据表的所有结果
 $article = App\\Article::where('article',1)
 ->orderBy('name','desc')
 ->take(10)
 ->get();
 foreach($articles as $article){
 echo $article->content;
 }
 ##################
 findOrFail()、firstOrFail()方法会取回查询的第一个结果。如果没有找到相应结果，则会抛出一个异常：
 $model = App\\Article::findOrFail(1);

在这出了个巨98恐怖的巨低级的错误... use APP(App) 使用模型来进行数据库交互：

 use App\\Article;//App不是APP！！！！
 public function store(Request $request)
 {
​
 $article = Article::create(\[
 'title' => $request->title,
 'content' => $request->content,
 'num'=>5,
 'source'=>'bank acount'
 \]);
​
 return redirect()->route('articles.index');
 }

#### 路由

##### 基本路由

最基本的路由只接收一个URL和一个闭包函数。

Route::get('hello',function(){
 return "hello world";
})

##### 路由分类

*   web路由：提供了CSRF保护和Session等功能
    
*   api路由：无状态的
    
    分配到不同的中间件
    

##### 常用的路由方法

Route::get('/net','net\\NetController@index');
Route::post($url,$callback);
Route::put($url,$callback);
Route::patch($url,$callback);
Route::delete($url,$callback);
Route::options($url,$callback);
###########################################
Route::match(\['get','post'\],'foo',function(){
 return "This is a request from get or post";
});
###any响应所有的http请求###
Route::any('bar', function () { 
 return 'This is a request from any HTTP verb';
});

##### CSRF保护

在 `routes/web.php` 路由文件中所有请求方式为 `PUT`、`POST` 或 `DELETE` 的路由对应的 HTML 表单都必须包含一个 CSRF 令牌字段，否则，请求会被拒绝。

<form method="POST" action="/profile">
 { { csrf_field() }}
 ...
</form>

##### 路由重定向

Route::redirect('/here(原路由)', '/there（定向到的路由）', 301);

##### 路由直接返回一个视图

Route::view('/welcome','welcome');
Route::view('/welcome', 'welcome', \['name' => 'name'\]);

##### 路由参数

*   必须参数
    

Route::get('user/{id}', function ($id) {
 return 'User ' . $id;
});

根据上面的示例，路由参数需要通过花括号 `{}` 进行包裹并且是拼音字母，这些参数在路由被执行时会被传递到路由的闭包。路由参数名称不能包含 `-` 字符，如果需要的话可以使用 `_` 替代，比如如果某个路由参数定义成 `{post-id}` 则访问路由会报错，应该修改成 `{post_id}` 才行。路由参数被注入到路由回调/控制器取决于它们的顺序，与回调/控制器名称无关。

*   可选参数
    
    有必选参数就有可选参数，这可以通过在参数名后加一个 `?` 标记来实现，这种情况下需要给相应的变量指定默认值，当对应的路由参数为空时，使用默认值：
    
    Route::get('user/{name?}', function ($name = null) {
     return $name;
    });
    ​
    Route::get('user/{name?}', function ($name = 'John') {
     return $name;
    });
    
*   正则约束
    
    可以通过路由实例上的where方法来约束路由参数的格式。where方法接收参数名和一个正则表达式来定义该参数如何被约束
    
    Route::get('user/{name}', function ($name) {
     // $name 必须是字母且不能为空
    })->where('name', '\[A-Za-z\]+');
    ​
    Route::get('user/{id}', function ($id) {
     // $id 必须是数字
    })->where('id', '\[0-9\]+');
    ​
    Route::get('user/{id}/{name}', function ($id, $name) {
     // 同时指定 id 和 name 的数据格式
    })->where(\['id' => '\[0-9\]+', 'name' => '\[a-z\]+'\]);
    ###id为可选参数，默认为killer
    Route::get('/testZZ/{id?}',function($id="killer"){
     return $id;
    })->where('id','\[0-9\]+');//正则限制id只能为数字
    
*   全局约束
    
    如果想要路由参数在全局范围内被给定正则表达式约束，可以使用 `pattern` 方法。需要在 `RouteServiceProvider` 类的 `boot` 方法中定义这种约束模式：
    
    /**
     \* 定义路由模型绑定，模式过滤器等
     *
     \* @param  \\Illuminate\\Routing\\Router  $router
     \* @return void
     \* @translator  http://laravelacademy.org
     */
    public function boot()
    {
     Route::pattern('id', '\[0-9\]+');
     parent::boot();
    }
    

##### 命名路由

命名路由为生成 URL 或重定向提供了方便，实现起来也很简单，在路由定义之后使用 `name` 方法链的方式来定义该路由的名称：

Route::get('user/profile', function () {
 // 通过路由名称生成 URL
 return 'my url: ' . route('profile');
})->name('profile');

还可以为控制器动作指定路由名称：

Route::get('user/profile', 'UserController@showProfile')->name('profile');

这样我们就可以通过以下方式定义重定向：

Route::get('redirect', function() {
    // 通过路由名称进行重定向
    return redirect()->route('profile');
});

如果命名路由定义了参数，可以将该参数作为第二个参数传递给 `route` 函数。给定的路由参数将会自动插入到 URL 中：

Route::get('user/{id}/profile', function ($id) {
    $url = route('profile', \['id' => 1\]);
    return $url;
})->name('profile');

**检查当前路由** 如果你想要判断当前请求是否被路由到给定命名路由，可以使用 Route 实例上的 `named` 方法，例如，你可以从路由中间件中检查当前路由名称：

/\*\*
 \* 处理输入请求
 \*
 \* @param  \\Illuminate\\Http\\Request  $request
 \* @param  \\Closure  $next
 \* @return mixed
 */
public function handle($request, Closure $next)
{
    if ($request->route()->named('profile')) {
        //
    }

    return $next($request);
}

##### 路由分组

###### 中间件

要给某个路由分组中定义的所有路由分配中间件，可以在定义分组之前使用 `middleware` 方法。中间件将会按照数组中定义的顺序依次执行：

Route::middleware(\['first', 'second'\])->group(function () {
    Route::get('/', function () {
        // Uses first & second Middleware
    });

    Route::get('user/profile', function () {
        // Uses first & second Middleware
    });
});

关于中间件的使用我们在后面单独讲[中间件](http://laravelacademy.org/post/8739.html)时再进行示例演示，这里我们先了解这样使用就行。

###### 命名空间

路由分组另一个通用的例子是使用 `namespace` 方法分配同一个 PHP 命名空间给该分组下的多个控制器：

Route::namespace('Admin')->group(function () {
    // Controllers Within The "App\\Http\\Controllers\\Admin" Namespace
});

默认情况下，`RouteServiceProvider` 在一个命名空间分组下引入所有路由文件，并指定所有控制器类所在的默认命名空间是 `App\Http\Controllers`，因此，我们在定义控制器的时候只需要指定命名空间 `App\Http\Controllers` 之后的部分即可。 关于命名空间后面我们单独讲控制器的时候还会再详细演示，这里先了解用法即可。

###### 子域名路由

路由分组还可以被用于处理子域名路由，子域名可以像 URI 一样被分配给路由参数，从而允许捕获子域名的部分用于路由或者控制器，子域名可以在定义分组之前调用 `domain` 方法来指定：

Route::domain('{account}.blog.dev')->group(function () {
    Route::get('user/{id}', function ($account, $id) {
        return 'This is ' . $account . ' page of User ' . $id;
    });
});

比如我们设置会员子域名为 `account.blog.test`，那么就可以通过 `http://account.blog.test/user/1` 访问用户ID为 `1` 的会员信息了：

This is account page of User 1

###### 路由前缀

`prefix` 方法可以用来为分组中每个路由添加一个给定 URI 前缀，例如，你可以为分组中所有路由 URI 添加 `admin` 前缀 ：

Route::prefix('admin')->group(function () {
    Route::get('users', function () {
        // Matches The "/admin/users" URL
    });
});

这样我们就可以通过 `http://blog.test/admin/users` 访问路由了。

###### 路由名称前缀

`name` 方法可通过传入字符串为分组中的每个路由名称设置前缀，例如，你可能想要在所有分组路由的名称前添加 `admin` 前缀，由于给定字符串和指定路由名称前缀字符串完全一样，所以需要在前缀字符串末尾后加上 `.` 字符：

Route::name('admin.')->group(function () {
    Route::get('users', function () {
        // 新的路由名称为 "admin.users"...
    })->name('users');
});

##### 自动路由，常用于api

`Route::resource('articles','ArticleController');` 在routes.php中添加此语句，会自动生成路由：

方法

路由

Controller里的方法名

GET

/users

index

GET

/users/create

create

POST

/users

store

GET

/users/{user}

show

GET

/users/{user}/edit

edit

PUT

/users/{user}

update

DELETE

/users/{user}

destroy

然后要在Controller里自己创建相应的方法：

 $article = Article::create(\[
 'title' => $request->title,
 'content' => $request->content,
 'num'=>5,
 'source'=>'bank acount'
 \]);
​
 return redirect()->route('articles.index');
 }

 ```php

 ```

 public function show($id) {
 echo $id;
 }
​
 public function edit($id) {}

 public function update($id) {}
 public function create()
 {
 return view('articles.create');
 }

##### 路由模型绑定

*   隐式绑定
    
    Route::get('article/{article}',function (App\\Article $article){
    	return $article;
    });
    
    访问：[http://localhost:8000/api/article/1](http://localhost:8000/api/article/1)
    
    得到：
    
    {"id":1,"content":"dzdTdyGV5rsjoyFsjvVbdYfDq97GQleeneUSvYRpWDovBd20ySvfMa9u2Lb3Q3jpbauj9K45FcBo3ckN","num":31,"source":"bank acount","created\_at":"2018-04-29 16:41:59","updated\_at":"2018-04-29 16:41:59","title":""}
    
*   显式绑定
    

##### 频率限制

Laravel 自带了一个中间件用于限制对应用路由的访问频率。开始使用该功能之前，分配 `throttle` 中间件到某个路由或路由分组，`throttle` 中间件接收两个参数用于判断给定时间内（单位：分钟）的最大请求次数。例如，我们指定登录用户每分钟只能访问下面的分组路由 60 次：

Route::middleware('auth:api', 'throttle:60,1')->group(function () {
    Route::get('/user', function () {
        //
    });
});

超出访问次数后，会返回 `429` 状态码并提示”Too many requests”。 **动态频率限制** 此外，还可以基于 `User` 模型的属性来动态设置最大请求次数。例如，如果 `User` 模型包含 `rate_limit` 属性，就可以将其这个属性名传递到 `throttle` 中间件，这样就可以将属性值作为计算最大请求次数的数据来源：

Route::middleware('auth:api', 'throttle:rate_limit,1')->group(function () {
    Route::get('/user', function () {
        //
    });
});

##### 表单方法伪造

HTML 表单不支持 `PUT`、`PATCH` 或者 `DELETE` 请求方法，因此，在 HTML 表单中调用 `PUT`、`PATCH` 或 `DELETE` 路由时，需要添加一个隐藏的 `_method` 字段，其值被用作该表单的 HTTP 请求方法：

<form action="/foo/bar" method="POST">
    <input type="hidden" name="_method" value="PUT">
    <input type="hidden" name="\_token" value="{ { csrf\_token() }}">
</form>

还可以直接使用 Blade 指令 `@method` 来生成 `_method` 字段：

<form action="/foo/bar" method="POST">
    @method('PUT')
    @csrf
</form>

##### 访问当前路由

你可以使用 `Route` 门面上的 `current`、`currentRouteName` 和 `currentRouteAction` 方法来访问处理当前输入请求的路由信息：

// 获取当前路由实例
$route = Route::current(); 
// 获取当前路由名称
$name = Route::currentRouteName();
// 获取当前路由action属性
$action = Route::currentRouteAction();