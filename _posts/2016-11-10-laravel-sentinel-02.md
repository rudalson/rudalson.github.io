---
layout: post
title:  "Laravel 5.3에서 Sentinel 활용 #2"
categories: PHP
tags: php laravel sentinel
---
유저를 등록하기 위해 post route를 작성한다.

## Route 등록
RegistrationController 에서 아래 추가. 등록을 마치면 root로 갈 수 있도록 작성
{% highlight php %}
public function postRegister(Request $request)
{
    $user = Sentinel::registerAndActivate($request->all());
    return redirect('/');
}
{% endhighlight %}

## link 등록
{% highlight php %}
...
<div class="panel-body">
    <form action="/register" method="POST">
        {{ csrf_field() }}
...
{% endhighlight %}
위처럼 action에 /register를 등록한다.

## Sentinel User 변경
위의 과정 이후 실제 유저를 등록하면 처음 등록했던 location 속성때문에 에러가 발생한다.
Sentinel의 User 클래스를 수정해줘야 한다.

먼저, config/cartalyst.sentinel.php 에서 보면
{% highlight php %}
...
'users' => [
    'model' => 'Cartalyst\Sentinel\Users\EloquentUser',
],
...
{% endhighlight %}
부분을 확인할 수 있다. 저 경로대로 EloquentUser 모델을 커스터마이징을 해줘야 한다.

{% highlight php %}
...
protected $fillable = [
        'email',
        'password',
        'last_name',
        'first_name',
        'permissions',
        'location',      // 추가
    ];
...
{% endhighlight %}


# 5 Login

## LoginController
{% highlight bash %}
php artisan make:controller LoginController
{% endhighlight %}

{% highlight php %}
public function login()
{
    return view('authentication.login');
}
{% endhighlight %}

## Route 추가
{% highlight php %}
Route::get('/login', 'LoginController@login');
{% endhighlight %}

## View 생성
{% highlight php %}
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
{% endhighlight %}

## post login 작성
web.php에서
{% highlight php %}
Route::get('/login', 'LoginController@login');
{% endhighlight %}

LoginController에서
{% highlight php %}
public function postLogin(Request $request)
{
    Sentinel::authenticate($request->all());

    return Sentinel::check();
}
{% endhighlight %}
