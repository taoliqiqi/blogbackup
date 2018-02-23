---
title: angular-ZERO
date: 2018-02-18 11:05:08
tags:
	- angular
categories: "Angular"
---

# 1.Input reference
这里**引用的是input对象** ，我们如果想传递**input的值**，可以用`usernameRef.value` ，然后就可以把 `onClick()` 方法改成 `onClick(usernameRef.value)`

```
  <input #usernameRef type="text">
  <button (click)="onClick(usernameRef.value)">Login</button>
```

# 2.Dependency Inject Service

如果不使用 **DI（依赖性注入)** 的时候，我们自然的想法是这样的，在 `login.component.ts` 中 import **引入** `AuthService`，在构造中 **初始化** `service` ，在 `onClick` 中 **调用** `service` 。
```javascript
import { Component, OnInit } from '@angular/core';
//引入AuthService
import { AuthService } from '../core/auth.service';

@Component({
  selector: 'app-login',
  template: `
    <div>
      <input #usernameRef type="text">
      <input #passwordRef type="password">
      <button (click)="onClick(usernameRef.value, passwordRef.value)">Login</button>
    </div>
  `,
  styles: []
})
export class LoginComponent implements OnInit {

  //声明成员变量，其类型为AuthService
  service: AuthService;

  constructor() {
    this.service = new AuthService();
  }

  ngOnInit() {
  }

  onClick(username, password) {
    //调用service的方法
    console.log('auth result is: ' + this.service.loginWithCredentials(username, password));
  }

}
```
这么做呢也可以跑起来，但存在几个问题：

1. 由于实例化是在组件中进行的，意味着我们如果更改service的构造函数的话，组件也需要更改。

2. 如果我们以后需要开发、测试和生产环境配置不同的AuthService，以这种方式实现会非常不方便。

**使用依赖注入**

1. 使用 **import** (`providers:[AuthService]`)

```javascript
import { Component, OnInit } from '@angular/core';
import { AuthService } from '../core/auth.service';

@Component({
  selector: 'app-login',
  template: `
    <div>
      <input #usernameRef type="text">
      <input #passwordRef type="password">
      <button (click)="onClick(usernameRef.value, passwordRef.value)">Login</button>
    </div>
  `,
  styles: [],
  //在providers中配置AuthService
  providers:[AuthService]
})
export class LoginComponent implements OnInit {
  //在构造函数中将AuthService示例注入到成员变量service中
  //而且我们不需要显式声明成员变量service了
  constructor(private service: AuthService) {
  }

  ngOnInit() {
  }

  onClick(username, password) {
    console.log('auth result is: ' + this.service.loginWithCredentials(username, password));
  }

}
```
2. 不**import** service

In `app.module.ts`

```javascript
providers: [
    {provide: 'auth',  useClass: AuthService}
    ]
```

In `login.component.ts`

```javascript
constructor(@Inject('auth') private service) {
  }
```
# 3.数据绑定
![image](https://penny-1256097328.cos.ap-beijing.myqcloud.com/WX20180218-121431%402x.png)

# 4.建立模拟web服务和异步操作

```
npm install --save angular-in-memory-web-api
```
创建`src\app\todo\todo-data.ts`

```javascript
import { InMemoryDbService } from 'angular-in-memory-web-api';
import { Todo } from './todo.model';

export class InMemoryTodoDbService implements InMemoryDbService {
  createDb() {
    let todos: Todo[] = [
      {id: "f823b191-7799-438d-8d78-fcb1e468fc78", desc: 'Getting up', completed: true},
      {id: "c316a3bf-b053-71f9-18a3-0073c7ee3b76", desc: 'Go to school', completed: false}
    ];
    return {todos};
  }
}
```