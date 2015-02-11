##HTTP Middleware http中间件

###Introduction 介绍

> HTTP middleware provide a convenient mechanism for filtering HTTP requests entering your application. For example, Laravel includes a middleware that verifies the user of your application is authenticated. If the user is not authenticated, the middleware will redirect the user to the login screen. However, if the user is authenticated, the middleware will allow the request to proceed further into the application.

> http中间件提供一种便捷的途径过滤进入应用的http请求。例如，laravel 包含了一个验证用户是否被认证的中间件，如果用户没有被认证，那么中间件将重定向用户到登陆界面。反之，如果用户被认证，将允许请求继续。

> Of course, middleware can be written to perform a variety of tasks besides authentication. A CORS middleware might be responsible for adding the proper headers to all responses leaving your application. A logging middleware might log all incoming requests to your application.

>当然，中间件除了用于认证还可以被用于多样化的场景。一个CORS 中间件可能在app返回响应信息时添加适当的头信息到全部的响应。一个中间件可能记录所有的请求信息

>There are several middleware included in the Laravel framework, including middleware for maintenance, authentication, CSRF protection, and more. All of these middleware are located in the app/Http/Middleware directory.

>框架中包含了维护、认证、CSRF保护等中间件。所有的这些中间件位于 app/Http/Middleware 目录

###Defining Middleware 定义中间件

>To create a new middleware, use the make:middleware Artisan command:

>创建一个新的中间件，可以使用Artisan command make:middleware  

    php artisan make:middleware OldMiddleware
> This command will place a new OldMiddleware class within your app/Http/Middleware directory. In this middleware, we will only allow access to the route if the supplied age is greater than 200. Otherwise, we will redirect the users back to the "home" URI.

>这个命令将在 app/Http/Middleware 目录下放置一个新的中间件类。在这个中间件中，我们将仅允许提交age大于200的请求进入路由，其他的将重定向到home 这个URI

    <?php namespace App\Http\Middleware;

        class OldMiddleware {

        /**
         * Run the request filter.
         *
         * @param  \Illuminate\Http\Request  $request
         * @param  \Closure  $next
         * @return mixed
         */
        public function handle($request, Closure $next)
        {
            if ($request->input('age') < 200)
            {
                return redirect('home');
            }
        
            return $next($request);
        }

    }

>As you can see, if the given age is less than 200, the middleware will return an HTTP redirect to the client; otherwise, the request will be passed further into the application. To pass the request deeper into the application (allowing the middleware to "pass"), simply call the $next callback with the $request.

>正如你所看到的，如果age小于200, 中间件将返回一个http重定向到客户端。相反，这个请求将能够进入应用，之后简单的执行了$next 这个回调函数。

>It's best to envision middleware as a series of "layers" HTTP requests must pass through before they hit your application. Each layer can examine the request and even reject it entirely.

>中间件就像是http请求到达你的应用时必须要通过的一些列层。每层都能检查请求甚至是完全拒绝它。

###Registering Middleware 注册中间件

* Global Middleware 全局中间件
>If you want a middleware to be run during every HTTP request to your application, simply list the middleware class in the $middleware property of your app/Http/Kernel.php class.

>如果你想一个中间件在每个http请求到你的应用期间被运行，简单的列出这个中间件类在 app/Http/Kernel.php class 的  $middleware 属性

    class Kernel extends HttpKernel {

        /**
         * The application's global HTTP middleware stack.
         *
         * @var array
         */
        protected $middleware = [
        	'Illuminate\Foundation\Http\Middleware\CheckForMaintenanceMode',
        	'Illuminate\Cookie\Middleware\EncryptCookies',
        	'Illuminate\Cookie\Middleware\AddQueuedCookiesToResponse',
        	'Illuminate\Session\Middleware\StartSession',
        	'Illuminate\View\Middleware\ShareErrorsFromSession',
        	'App\Http\Middleware\VerifyCsrfToken',
        ];
        
      /**
    	 * The application's route middleware.
    	 *
    	 * @var array
    	 */
    	protected $routeMiddleware = [
    		'auth' => 'App\Http\Middleware\Authenticate',
    		'auth.basic' => 'Illuminate\Auth\Middleware\AuthenticateWithBasicAuth',
    		'guest' => 'App\Http\Middleware\RedirectIfAuthenticated',
    	];
    }
    
* Assigning Middleware To Routes 分配中间件到路由

>If you would like to assign middleware to specific routes, you should first assign the middleware a short-hand key in your app/Http/Kernel.php file. By default, the $routeMiddleware property of this class contains entries for the middleware included with Laravel. To add your own, simply append it to this list and assign it a key of your choosing.

>如果你想分配中间件到具体的路由，你首先应该在app/Http/Kernel.php 文件里分配给这个中间件一个short-hand key(类似别名，易记的)。在laravel里默认的，$routeMiddleware 这个属性包含这些内容。添加自己的中间件，简单的可以直接在这个列表里追加并分配一个key (具体代码 参见上面的示例代码)

Once the middleware has been defined in the HTTP kernel, you may use the middleware key in the route options array:

一旦这个中间件在in the HTTP kernel被定义，你就可以通过这个中间件的key，在路由的可选数组中使用。

    Route::get('admin/profile', ['middleware' => 'auth', function()
    {
        //
    }]);

* Terminable Middleware 可终止的中间件
>Sometimes a middleware may need to do some work after the HTTP response has already been sent to the browser. For example, the "session" middleware included with Laravel writes the session data to storage after the response has been sent to the browser. To accomplish this, you may define the middleware as "terminable".

>有些时候，在一个http响应已经准备好发送到浏览器后，中间件可能还需要做一些工作。例如，laravel自带的'session'中间件在响应发送到浏览器之后还需要将session数据写入存储。为了实现这些，你可能需要定义类似"terminable"的中间件

    use Illuminate\Contracts\Routing\TerminableMiddleware;
    
    class StartSession implements TerminableMiddleware {
    
        public function handle($request, $next)
        {
            return $next($request);
        }
    
        public function terminate($request, $response)
        {
            // Store the session data...
        }
    
    }

>As you can see, in addition to defining a handle method, TerminableMiddleware define a terminate method. This method receives both the request and the response. Once you have defined a terminable middleware, you should add it to the list of global middlewares in your HTTP kernel.

正如你看到的，除了定义一个handle方法外，TerminableMiddleware定义了一个终结方法。这个方法接受
request和response 2参数。一旦你定义一个可终结的中间件，你应该添加它到全局的中间件列表。