##Configuration 配置

* <a href="#introduction">Introduction 简介</a>
* <a href="#after">After Installation 安装之后</a>
* <a href="#acvalue">Accessing Configuration Values 访问配置项</a>
* <a href="#env">Environment Configuration 环境配置</a>
* <a href="#configcache">Configuration Caching 配置缓存</a>
* <a href="#mmode">Maintenance Mode 维护模式</a>
* <a href="#prettyurls">Pretty URLs 优雅的链接</a>
### <a name="introduction">Introduction 简介</a>

>All of the configuration files for the Laravel framework are stored in
 the config directory. Each option is documented, so feel free to look 
 through the files and get familiar with the options available to you.

>Laravel框架的所有的配置文件都被存放在config目录。每个选项都被文档化了，
你可以轻松的浏览配置文件并且熟悉各选项的用途。

### <a name="after">After Installation 安装之后</a>

* Naming Your Application 命名你的应用

> After installing Laravel, you may wish to "name" your application. 
> By default, the app directory is namespaced under App, 
> and autoloaded by Composer using the PSR-4 autoloading standard. 
> However, you may change the namespace to match the name of your 
> application, which you can easily do via the app:name Artisan command.

> Laravel安装完成之后，你可能希望给应用起个名字。一般，app的命名空间是App，
> 并且基于comporser使用的PSR-4标准自动加载。当然，你也可以很容易的通过
> Artisan的命令 app::name改变命名空间以匹配你的应用。

> For example, if your application is named "Horsefly", you could run 
> the following command from the root of your installation:

> 例如，你的应用被命名为"Horsefly",你可以在你应用的安装目录运行下面的命令：
    
    php artisan app:name Horsefly

> Renaming your application is entirely optional, and you are free to keep 
> the App namespace if you wish.

> 重命名应用是完全可选的，如果你愿意，你可以自由的App命名空间。


* Other Configuration 其他的配置

>Laravel needs very little configuration out of the box. You are free to 
get started developing! However, you may wish to review the config/app.php file 
and its documentation. It contains several options such as timezone and locale 
that you may wish to change according to your location.

>Laravel 只需要很少的额外配置，就可以很容易的开始开发。但是，你可能希望检查config/app.php
文件以及它的配置内容。它包含了一些列你可能希望和你的项目保持一直配置项，诸如时区和本地化。

>Once Laravel is installed, you should also configure your local environment.

>一旦Laravel被安装，你应该配置你的本地环境。

    Note: You should never have the app.debug configuration option 
    set to true for a production application.
    
    提示:千万不要在生产环境设置app.debug＝true

* Permissions

>Laravel may require one set of permissions to be configured: folders 
within storage require write access by the web server.

>Laravel可能需要配置权限:你的web服务器需要对在storage目录之内的文件夹的有写权限。

###<a name="acvalue">Accessing Configuration Values  访问配置项的值</a>

> You may easily access your configuration values using the Config facade:

> 使用Config facade,你能很容易的访问配置项的值。

    $value = Config::get('app.timezone');

    Config::set('app.timezone', 'America/Chicago');

> You may also use the config helper function:

> 你也可以使用config 帮助函数

    $value = config('app.timezone');
      

###<a name="env">Environment Configuration 环境配置</a> 

>It is often helpful to have different configuration values based 
on the environment the application is running in. For example, you may wish to 
use a different cache driver locally than you do on your production server. 
It's easy using environment based configuration.

>这对基于应用运行环境而有不同配置的情况特别有帮助。例如，你可能希望在本地和在
生产环境使用不同的缓存驱动。这很容易使用基于环境的配置。

>To make this a cinch, Laravel utilizes the DotEnv PHP library by Vance Lucas. 
In a fresh Laravel installation, the root directory of your application will 
contain a .env.example file. If you install Laravel via Composer, 
this file will automatically be renamed to .env. Otherwise, 
you should rename the file manually.

>为了使这些变得简单，Laravel使用了Vance Lucas的DotEnv库。在一个新的Laravel安装的时候，
应用根目录将包含一个 .env.example 文件。如果你通过Composer安装，这个文件将自动重命名为.env。
否则，你应该手动的重命名该文件。

>All of the variables listed in this file will be loaded into the $_ENV 
PHP super-global when your application receives a request. 
You may use the env helper to retrieve values from these variables. 
In fact, if you review the Laravel configuration files, you will notice 
several of the options already using this helper!

>当你的应用接受一个请求的时候，所有在.env文件里的列出的变量都将被加载到php的$_ENV
这个超全局变量里。你可以用env这个助手函数来检索这些变量的值。实际上，
如果你看一下Laravel的配置文件，你将会发现一些选项已经在使用这个助手函数了

>Feel free to modify your environment variables as needed for your 
own local server, as well as your production environment. 
However, your .env file should not be committed to your application's 
source control, since each developer / server using your application 
could require a different environment configuration.

> 和修改生产环境的变量一样，可以根据本地服务的需要随时修改你的环境变量。
> 但是，你的.env 文件不应该被提交到应用程序源代码的版本控制系统中。
> 自此，每一个使用这个应用的开发者或者服务器可以包含不同的环境配置。

>If you are developing with a team, you may wish to continue including 
a .env.example file with your application. By putting place-holder values 
in the example configuration file, other developers on your team 
can clearly see which environment variables are needed to run your application.

> 如果在一个团队中从事开发，你可能仍旧希望的包含一个.env.example文件在您的应用中。通过在示例配置文件放置占位的值，团队中的其他开发者能够清晰的看到运行应用所需要的环境配置。

* Accessing The Current Application Environment 访问当前应用的环境

>You may access the current application environment via the environment method on the Application instance:

>你可以通过Application实例的environment方法访问当前应用的环境。

    $environment = $app->environment();
    
>You may also pass arguments to the environment method to check if the environment matches a given value:

你也可以通过给environment方法传入参数来确认环境是否和一个被给定的值相匹配。


    if ($app->environment('local'))
    {
        // The environment is local
    }
    
    if ($app->environment('local', 'staging'))
    {
        // The environment is either local OR staging...
    }
    
>To obtain an instance of the application, resolve the Illuminate\Contracts\Foundation\Application contract via the service container. Of course, if you are within a service provider, the application instance is available via the $this->app instance variable.

>通过<a href="#servicecontainer">service container</a>解析 the `Illuminate\Contracts\Foundation\Application` 这个contract, 从而获得一个application的实例。当然，如果在一个<a href="#serviceprovider">service provider</a>内部，通过`$this->app`这个实例变量得到的应用的实例是有效的。

>An application instance may also be accessed via the app helper of the App facade:

>一个应用的实例也可能通过App facade的app()助手函数被访问。

    $environment = app()->environment();

    $environment = App::environment();

###<a name="configcache">Configuration Caching 配置缓存</a>

>To give your application a little speed boost, you may cache all of your configuration files into a single file using the config:cache Artisan command. This will combine all of the configuration options for your application into a single file which can be loaded quickly by the framework.

>你可以使用Artisan命令config:cache将应用所有的配置文件缓存到一个文件中，这样可以提升应用的速度这样为你的应用合并所有的配置项到一个文件，可以使它被框架快速的加载。

>You should typically run the config:cache command as part of your deployment routine.

>日常部署应用的时候应该按照惯例的执行config:cache命令。

###<a name="mmode">Maintenance Mode 维护模式</a>

>When your application is in maintenance mode, a custom view will be displayed for all requests into your application. This makes it easy to "disable" your application while it is updating or when you are performing maintenance. A maintenance mode check is included in the default middleware stack for your application. If the application is in maintenance mode, an HttpException will be thrown with a status code of 503.

>当你的应用在维护模式时，一个定制的视图将被显示给所有进入应用的请求。当应用正在升级或者维护的时候
这种模式使应用"disable"变得简单。在默认的中间件堆栈中确认维护模式是否被包含。如果应用在维护模式下，一个http异常将被抛出，状态码503.

>To enable maintenance mode, simply execute the down Artisan command:

>开启维护模式，只要执行Artisan命令 down

    php artisan down
>To disable maintenance mode, use the up command:

>结束维护模式，使用up命令

    php artisan up

* Maintenance Mode Response Template 维护模式响应的模板

>The default template for maintenance mode responses is located in resources/views/errors/503.blade.php.

>默认的维护模式模板被放在 `resources/views/errors/503.blade.php`。


* Maintenance Mode & Queues 维护模式和队列

>While your application is in maintenance mode, no queued jobs will be handled. The jobs will continue to be handled as normal once the application is out of maintenance mode.

>当应用开启维护模式，队列的jobs将不再被处理，直到关闭维护模式。

###<a name="prettyurls">Pretty URLs 优雅的链接</a>

* Apache

>The framework ships with a public/.htaccess file that is used to allow URLs without index.php. If you use Apache to serve your Laravel application, be sure to enable the mod_rewrite module.

>框架在public目录下提供了一个.htaccess文件用于允许urls不带index.php。如果你使用apache作为Laravel的web服务器，请确认mod_rewrite模块是否可用。


>If the .htaccess file that ships with Laravel does not work with your Apache installation, try this one:

>如果laravel提供的.htaccess文件不能正常工作，请尝试下面的写法

    Options +FollowSymLinks
    RewriteEngine On

    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteRule ^ index.php [L]

* Nginx

>On Nginx, the following directive in your site configuration will allow "pretty" URLs:

>在Nginx上，将下列指令放入你的站点配置中，即可开启Pretty Urls

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }
>Of course, when using Homestead, pretty URLs will be configured automatically.

>通常，当使用Homestead的时候，Pretty URLs将被自动配置


