---
layout: post
title:  "Laravel with Sentinel - 03. Registration"
categories: PHP
tags: php laravel sentinel
---
[Laravel 5.3 advanced Authentication #3 Registration](https://www.youtube.com/watch?v=xvlA1OSzZ5k&list=PL3ZhWMazGi9KB9PajJHWvV2NJ1ITNoNGp&index=3) 참조

## Migration 수정

#### 2014_07_02_230147_migration_cartalyst_sentinel.php
{% highlight php %}
Schema::create('users', function (Blueprint $table)
{
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

## Controller 생성
{% highlight bash %}
php artisan make:controller RegistrationController
{% endhighlight %}

#### routes/web.php
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
{% highlight html %}{% raw %}
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
{% endraw %}{% endhighlight %}