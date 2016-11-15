---
layout: post
title:  "Laravel 5.3에서 Sentinel 활용 #3"
categories: PHP
tags: php laravel sentinel
---
# #6 Menu

## Layout 구성

Bootstrap에서 Narrow Jumbotron template를 가져와서 `layouts/master.blade.php`에 만든다.

## master.blade.php
```php
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Authentication</title>

    <!-- Bootstrap core CSS -->
    <link href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">

    <!-- Custom styles for this template -->
    <link href="/css/jumbotron-narrow.css" rel="stylesheet">

    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css">
  </head>

  <body>

    <div class="container">
        @include('layouts.top-menu')
        @yield('content')
    </div> <!-- /container -->
  </body>
</html>
```

## top.menu.php
```php
<div class="header clearfix">
  <nav>
    <ul class="nav nav-pills pull-right">
      <li role="presentation"><a href="/login">Login</a></li>
      <li role="presentation"><a href="/register">Register</a></li>
    </ul>
  </nav>
  <h3 class="text-muted">Authentication with Sentinel</h3>
</div>
```

## login, register blade 수정
이제 layout을 bootstrap로 잡았으니 이를 적용한다.

### login.blade.php & register.blade.php
2파일 제거할 때 앞부분 `div container`까지 제거한다.

```php
@extends('layouts.master')

@section('content')
    <div class="row">
        <div class="col-md-6 col-md-offset-3">
            <div class="panel panel-primary">
...
    </div>
@endsection
```


# #7 Logout & show content to logged users only

## logout 구현

### web.php
```php
Route::post('/logout', 'LoginController@logout');
```

### LoginController
```php
public function logout()
{
    Sentinel::logout();
    return redirect('/login');
}
```

## top.menu.php 수정
```php
<div class="header clearfix">
  <nav>
    <ul class="nav nav-pills pull-right">
      @if(Sentinel::check())
        <li role="presentation">
          <form action="/logout" method="POST" id="logout-form">
            {{ csrf_field() }}
            <a href="#" onclick="document.getElementById('logout-form').submit()">Logout</a>
          </form>
        </li>
      @else
        <li role="presentation"><a href="/login">Login</a></li>
        <li role="presentation"><a href="/register">Register</a></li>
      @endif
    </ul>
  </nav>
  <h3 class="text-muted">
    @if(Sentinel::check())
      Hello, {{ Sentinel::getUser()->first_name }}
    @else
      Authentication with Sentinel
    @endif
  </h3>
</div>
```
