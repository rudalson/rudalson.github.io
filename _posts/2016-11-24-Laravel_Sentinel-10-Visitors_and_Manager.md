---
layout: post
title:  "Laravel with Sentinel - 10. Visitors & Manager"
categories: PHP
tags: php laravel sentinel
---
[Laravel 5.3 advanced Authentication #10 Visitors & Manager](https://www.youtube.com/watch?v=LgrqIsyJHVk) 참조

## Visitor 권한
우선 `VisitorMiddleware`를 만든다.

```bash
php artisan make:middleware VisitorsMiddleware
```

#### routes/web.php
```php
Route::group(['middleware' => 'visitors'], function() {
    Route::get('/register', 'RegistrationController@register');
    Route::post('/register', 'RegistrationController@postRegister');

    Route::get('/login', 'LoginController@login');
    Route::post('/login', 'LoginController@postLogin');
});
```
visitor가 접근 할 수 있는 `register`, `login` 경로는 `visitor middleware group`로 묶는다.

#### App/Http/Kernel.php
```php
...
'visitors' => \App\Http\Middleware\VisitorsMiddleware::class,
```

#### App/Http/Middleware/VisitorsMiddleware
```php
use Sentinel;
...
public function handle($request, Closure $next)
{
    if(!Sentinel::check())
        return $next($request);

    return redirect('/');
}
```

## earnings view

#### resources/views/admins/earnings.blade.php
{% highlight html %}{% raw %}
Total earnings 9999

<form action="/logout" method="POST" id="logout-form">
    {{ csrf_field() }}
    <a href="#" onclick="document.getElementById('logout-form').submit()">Logout</a>
</form>
{% endraw %}{% endhighlight %}

#### App/Http/Controllers/AdminController
```php
public function earnings()
{
    return view('admins.earnings');
}
```

## Manager 권한
```bash
php artisan make:middleware ManagerMiddleware
```

#### App/Http/Kernel.php
```php
...
'manager' => \App\Http\Middleware\ManagerMiddleware::class,
```

#### App/Http/Middleware/ManagerMiddleware
```php
use Sentinel;
...
public function handle($request, Closure $next)
{
    if (Sentinel::check() && Sentinel::getUser()->roles()->first()->slug == 'manager') {
        // \Log::info('role', ['role' => Sentinel::getUser()->roles()->first()]);
        return $next($request);
    }
    else
        return redirect('/');
}
```

#### routes/web.php
```php
...
Route::get('tasks', 'ManagerController@tasks')->middleware('manager');
```

#### App/Http/Controllers/ManagerController
```php
public function earnings()
{
    return view('managers.tasks');
}
```

## 로그인 시 권한에 맞는 페이지 redirect

#### LoginController
```php
public function postLogin(Request $request)
{
    Sentinel::authenticate($request->all());

    $slug = Sentinel::getUser()->roles()->first()->slug;

    if ($slug =='admin')
        return redirect('/earnings');
    elseif ($slug =='manager')
        return redirect('/tasks');

    return Sentinel::check();
}
```