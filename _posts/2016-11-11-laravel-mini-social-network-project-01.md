---
layout: post
title:  "Laravel 5.3으로 만들어보는 Mini Social Network"
categories: PHP
tags: php laravel
---
[Devlov의 강의](https://goo.gl/ND53L9)를 보고 내용 정리

우선 라라벨 프로젝트를 생성
# 23. Social Network Custom Registration

## Migrate
{% highlight bash %}
php artisan migrate
{% endhighlight %}

## Auth
{% highlight bash %}
php artisan make:auth
{% endhighlight %}

## 유저 항목 customizing
{% highlight php %}
public function up()
{
    Schema::create('users', function (Blueprint $table) {
        $table->increments('id');
        $table->string('username', 32); // 추가
        $table->date('dob');            // 추가
        $table->string('name');
        $table->string('email')->unique();
        $table->string('password');
        $table->rememberToken();
        $table->timestamps();
    });
}
{% endhighlight %}
위의 두 항목 추가


# 24. Social Network Change Custom Authentication
이전 단계에서 db users 테이블에서 2개항목을 추가하였기에 관련 내용을 적용한다.
nullable()이나 default()로 줘도 되지만 유저 입력창을 생성한다.

## User Input 추가
{% highlight php %}
<div class="form-group{{ $errors->has('username') ? ' has-error' : '' }}">
    <label for="username" class="col-md-4 control-label">Username</label>

    <div class="col-md-6">
        <input id="username" type="text" class="form-control" name="username" value="{{ old('username') }}" required autofocus>

        @if ($errors->has('username'))
            <span class="help-block">
                <strong>{{ $errors->first('username') }}</strong>
            </span>
        @endif
    </div>
</div>

<div class="form-group{{ $errors->has('dob') ? ' has-error' : '' }}">
    <label for="dob" class="col-md-4 control-label">dob</label>

    <div class="col-md-6">
        <input id="dob" type="date" class="form-control" name="dob" value="{{ old('dob') }}" required autofocus>

        @if ($errors->has('dob'))
            <span class="help-block">
                <strong>{{ $errors->first('dob') }}</strong>
            </span>
        @endif
    </div>
</div>
{% endhighlight %}

## Controller에 항목 추가
{% highlight php %}
protected function create(array $data)
{
    return User::create([
        'name' => $data['name'],
        'email' => $data['email'],
        'password' => bcrypt($data['password']),
        'username' => $data['username'],        // 추가
        'dob' => $data['dob'],                  // 추가
    ]);
}
{% endhighlight %}

## Model fillabe 추가
{% highlight php %}
protected $fillable = [
    'name', 'email', 'password', 'username', 'dob',
];
{% endhighlight %}
username과 dob 추가
