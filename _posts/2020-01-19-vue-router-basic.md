---
layout: post
title:  "vue router 기초"
categories: frontend
tags: vue router frontend
---

`SPA`에서는 sub path에 대한 관리를 별도로 해줘야 한다. 말그대로 single page 이므로 이후의 page는 없는 것이다. 이를 위해 vue 에서는 `vue-router` 를 사용한다.

## router 내용 선언

vue-router가 SPA에서 다른 경로의 `component`를 가르키는 기본 형태는 아래와 같다.

*router/index.js*
```js
{
    path: '/about',
    name: 'about',
    // route level code-splitting
    // this generates a separate chunk (about.[hash].js) for this route
    // which is lazy-loaded when the route is visited.
    component: () => import(/* webpackChunkName: "about" */ '../views/About')
  }
```
위의 주석을 살펴보면 SPA이므로 처음 로딩부터 모든 component 요소를 가지고 있기보다는 `lazy-loaded`를 하기 위해 `() function`으로 감싼 것을 볼수 있다. 그리고 그 내용은 `router/index.js` 경로에서 부터 상대경로를 넣어주면 된다. 

## route 시키기
이제 `about` 컴포넌트를 보여주기 위해서는 기존처럼 `href` 같은 형태가 아닌 vue-route의 기능을 사용하여 호출되어야 한다. 그러기 위해 drawer의 template에서 예를 살펴본다.

```js
<v-list-item link>
    <v-list-item-action>
        <v-icon>mdi-contact-mail</v-icon>
    </v-list-item-action>
    <v-list-item-content>
        <v-list-item-title>About contact</v-list-item-title>
    </v-list-item-content>
</v-list-item>
```

### 1. $router.push() 사용하기
```js
<v-list-item link @click="$router.push('/about')">
```
이런 형태로 직접 component에 명시된 /about 경로를 바로 가르킬 수 있다.

```js
<v-list-item link @click="$router.push({name: 'about'})">
```
name이 about인 component를 찾아서 경로로 가르킬 수 있다. 이렇게 사용할 경우는 뒤에 parameter를 추가로 넣어 줄 수 있는 장점 뿐만 아니라 조금 더 명시적으로 가르켜 사용하도록 해준다.


### 2. key-value 값으로 component 찾아가기
또 다른 방법으로는 아래와 같은 문법도 있다.
*vue* file
```vue
router :to="{name: 'home'}"
```

혹은 `router-link` 를 사용할 수 있다.
```
<router-link :to="{name: 'home'}">
```