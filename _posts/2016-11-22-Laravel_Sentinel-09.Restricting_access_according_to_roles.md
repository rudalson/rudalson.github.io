---
layout: post
title:  "Laravel with Sentinel - 09. Restricting Access According to Roles"
categories: PHP
tags: php laravel sentinel
---
[Laravel 5.3 advanced Authentication #9 Restricting Access According to Roles](https://www.youtube.com/watch?v=KoaX5N840Ao&list=PL3ZhWMazGi9KB9PajJHWvV2NJ1ITNoNGp&index=9) 참조

## Admin Controller 추가
```bash
php artisan make:controller AdminController
```

#### routes/web.php
```php
Route::get('earnings', 'AdminController@earnings');
```

#### AdminController
```php
public function earnings()
{
    return 'Total earnings 999';
}
```

## Admin Middleware 추가
```bash
php artisan make:middleware AdminMiddleware
```

#### App/Http/Middleware/AdminMiddleware.php
```php
use Sentinel;
...
public function handle($request, Closure $next)
{
    // 1. User should be authenticated

    // 2. Authenticated user should be an admin

    if (Sentinel::check())
        return $next($request);

    return redirect('/');
}
```

#### App/Http/Kernel.php
```php
...
protected $routeMiddleware = [
    ...
    'admin' => \App\Http\Middleware\AdminMiddleware::class,
];
```

#### routes/web.php
```php
Route::get('earnings', 'AdminController@earnings')->middleware('admin');
```

여기까지 했지만 `earnings` 페이지에 가보면 모든 계정이 다 접근이 가능하다. 이에 대해 restrict를 걸어보겠다.

## restrict 걸기

### log 사용하기

#### App/Http/Middleware/AdminMiddleware.php
```php
if (Sentinel::check()) {
    \Log::info('role', ['role' => Sentinel::getUser()->roles()->first()]);
    return $next($request);
}
```
로그를 찍는 것은 `Log::info()`를 사용하면 된다. 로그를 찍게 되면 `storage/logs` 디렉토리 밑에 `laravel.log`에 남게 된다.

현재는 log를 사용하여 Sentinel에서 가져오는 user role 정보를 확인해본다.
