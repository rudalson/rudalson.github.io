---
layout: post
title:  "Laravel with Sentinel - 05. Login"
categories: PHP
tags: php laravel sentinel
---
[Laravel 5.3 advanced Authentication #5 Login](https://www.youtube.com/watch?v=zOvsxmSE_D4&index=5&list=PL3ZhWMazGi9KB9PajJHWvV2NJ1ITNoNGp) 참조

## LoginController 생성
{% highlight bash %}
php artisan make:controller LoginController
{% endhighlight %}

#### LoginController
{% highlight php %}
public function login()
{
    return view('authentication.login');
}
{% endhighlight %}

## Route 추가

#### web.php
{% highlight php %}
Route::get('/login', 'LoginController@login');
{% endhighlight %}

## View 생성
{% highlight html %}{% raw %}
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css">
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css">

<div class="container">
    <div class="row">
        <div class="col-md-6 col-md-offset-3">
            <div class="panel panel-primary">
                <div class="panel-heading">
                    <h3 class="panel-title"> Login </h3>
                </div>

                <div class="panel-body">
                    <form action="/login" method="POST">
                        {{ csrf_field() }}

                        <div class="form-group">
                            <div class="input-group">
                                <span class="input-group-addon"><i class="fa fa-envelope"></i></span>
                                <input type="email" name="email" class="form-control" placeholder="example@example.com" required>
                            </div>
                        </div>

                        <div class="form-group">
                            <div class="input-group">
                                <span class="input-group-addon"><i class="fa fa-lock"></i></span>
                                <input type="password" name="password" class="form-control" placeholder="Password" required>
                            </div>
                        </div>

                        <div class="form-group">
                            <input type="submit" value="Login" class="btn btn-success pull-right">
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</div>
{% endraw %}{% endhighlight %}

## post login 작성

#### web.php
{% highlight php %}
Route::get('/login', 'LoginController@login');
{% endhighlight %}

#### LoginController
{% highlight php %}
public function postLogin(Request $request)
{
    Sentinel::authenticate($request->all());

    return Sentinel::check();
}
{% endhighlight %}
