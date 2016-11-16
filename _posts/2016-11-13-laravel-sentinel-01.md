---
layout: post
title:  "Laravel 5.3에서 Sentinel 활용 #1"
categories: PHP
tags: php laravel sentinel
---
'Sentinel' 은 'auth' 관련 패키지이다. 이것을 Laravel 5.3에서 사용하는 방법을 정리해본다.

# 2. Setting Up
Sentinel By Cartalyst 검색

## Install by Composer
```bash
composer require cartalyst/sentinel "2.0.*"
```

## Integration
config/app.php
$providers

{% highlight php %}
Cartalyst\Sentinel\Laravel\SentinelServiceProvider::class,

$aliases
'Activation' => Cartalyst\Sentinel\Laravel\Facades\Activation::class,
'Reminder'   => Cartalyst\Sentinel\Laravel\Facades\Reminder::class,
'Sentinel'   => Cartalyst\Sentinel\Laravel\Facades\Sentinel::class,
{% endhighlight %}

## Assets
{% highlight bash %}
php artisan vendor:publish --provider="Cartalyst\Sentinel\Laravel\SentinelServiceProvider"
{% endhighlight %}

이후 config/cartalyst.sentinel.php 와 migration file 추가

## Migrations
{% highlight bash %}
php artisan migrate
{% endhighlight %}
이 때 default user migration이 있었다면 rollback 후 다시 할 것

# 3. Registration
## Migration 수정

2014_07_02_230147_migration_cartalyst_sentinel.php 에서

{% highlight php %}
Schema::create('users', function (Blueprint $table) {
...
    $table->string('first_name')->nullable();
    $table->string('last_name')->nullable();
    $table->string('location');     // 추가
    $table->timestamps();
...
{% endhighlight %}

{% highlight bash %}
php artisan migrate:refresh
{% endhighlight %}

## Controller
{% highlight bash %}
php artisan make:controller RegistrationController
{% endhighlight %}

routes/web.php 에서
{% highlight php %}
Route::get('/register', 'RegistrationController@register'); // 추가
{% endhighlight %}

App/Http/Controllers/RegisterationController.php
{% highlight php %}
public function register()
{
    return view('authentication.register');
}
{% endhighlight %}

## View
2번째 link FontAwesome 사용하고 나면 아이콘 사용 가능
{% highlight php %}
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css">
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css">

<div class="container">
    <div class="row">
        <div class="col-md-6 col-md-offset-3">
            <div class="panel panel-primary">
                <div class="panel-heading">
                    <h3 class="panel-title">Register</h3>
                </div>

                <div class="panel-body">
                    <form action="" method="POST">
                        {{ csrf_field() }}

                        <div class="form-group">
                            <div class="input-group">
                                <span class="input-group-addon"><i class="fa fa-envelope"></i></span>
                                <input type="email" name="email" class="form-control" placeholder="example@example.com">
                            </div>
                        </div>

                        <div class="form-group">
                            <div class="input-group">
                                <span class="input-group-addon"><i class="fa fa-user"></i></span>
                                <input type="text" name="first_name" class="form-control" placeholder="First Name">
                            </div>
                        </div>

                        <div class="form-group">
                            <div class="input-group">
                                <span class="input-group-addon"><i class="fa fa-user"></i></span>
                                <input type="text" name="last_name" class="form-control" placeholder="Last Name">
                            </div>
                        </div>

                        <div class="form-group">
                            <div class="input-group">
                                <span class="input-group-addon"><i class="fa fa-map-marker"></i></span>
                                <input type="text" name="location" class="form-control" placeholder="Location">
                            </div>
                        </div>

                        <div class="form-group">
                            <div class="input-group">
                                <span class="input-group-addon"><i class="fa fa-lock"></i></span>
                                <input type="password" name="password" class="form-control" placeholder="Password">
                            </div>
                        </div>

                        <div class="form-group">
                            <div class="input-group">
                                <span class="input-group-addon"><i class="fa fa-lock"></i></span>
                                <input type="password" name="password_confirmation" class="form-control" placeholder="Password Confirmation">
                            </div>
                        </div>

                        <div class="form-group">
                            <input type="submit" value="Register" class="btn btn-success pull-right">
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</div>
{% endhighlight %}
