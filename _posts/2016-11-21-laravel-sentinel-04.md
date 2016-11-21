---
layout: post
title:  "Laravel 5.3에서 Sentinel 활용 #4"
categories: PHP
tags: php laravel sentinel
---
# #8 Roles
[Laravel 5.3 advanced Authentication #8 Roles](https://www.youtube.com/watch?v=1S209IXvWVs) 참조

## roles 데이터 추가

Sentinel로 구성된 db에는 `roles` 라는 테이블이 존재한다. 여기에 2개의 data를 insert 하는데 `slug`, `name` column에

* admin, Admin
* manager, Manager

이렇게 2개의 data를 insert 한다. (영상에서는 phpmyadmin 사용)

## role 부여
user를 register 할 때 이전 단계에서 생성했던 role중 manager role을 주는 방법은

#### RegistrationController
```php
public function postRegister(Request $request)
{
    $user = Sentinel::registerAndActivate($request->all());

    $role = Sentinel::findRoleBySlug('manager');    // 추가
    $role->users()->attach($user);                  // 추가

    return redirect('/');
}
```

이렇게 한 후 user를 가입시켜 보면 `role_users` 테이블에 `user_id`와 `role_id가` 추가된 것을 확인할 수 있다.