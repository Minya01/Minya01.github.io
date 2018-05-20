---
layout: post
title:  typescript学习
date:   2018-05-21
categories: typescript
---

出差杭州一周，36度的大杭州真是热啊，这两天下雨一下子就降了12度，逛了西湖，烟雨蒙蒙，忍不住就哼唱一句“西湖的美，我的泪....”

废话不多说，收收心，继续努力工作。



TypeScript是一种强类型，面向对象的编译语言。TypeScript既是语言，也是工具。TypeScript是JavaScript类型的超集，可以编译成JavaScript。



### 为什么使用TypeScript

##### 优点

* __编译__ - JavaScript是解释型语言，需要运行才能知道是否有错。TYpeScript转换器在编译代码时可以进行错误检查，可以在运行之前进行错误检查。
* __强类型__ - JavaScript没有强类型，TypeScript可以约束类型。
* __面向对象__ - 支持类、接口、继承等。





### 安装

##### 安装TypeScript

```
sudo npm install -g typescript
```



##### 创建TypeScript文件

新建一个hello.ts文件：

```typescript
function hello(person) {
    return 'hello, ' + person;
}

let user = 'summer';

console.log(hello(user));
```



##### 编译代码

使用命令：

```typescript
tsc hello.ts
```

编译结果为一个hello.js文件，包含编译的JavaScript代码。



### 功能点

##### 类型注解

TypeScript可以给函数或变量添加类型。如：

```typescript
function hello(person: string) {
    return 'hello, ' + person;
}

let user = [1,2];

console.log(hello(user));
```

以上代码报错，提示number[]类型与string不匹配。



##### 接口

接口用于类型检查对象是否适合特定的结构。通过定义一个接口，可以命名一个特定的变量组合。

例如：

```typescript
// 定义Food接口
interface Food {
    name: string;
    calories : number;
}

// 函数接收Food
function speak(food: Food): void{
  console.log("Our " + food.name + " has " + food.calories + " calories.");
}

// 声明一个跟Food属性相同的变量
var ice_cream = {
    name:'ice cream',
    calories: 100
}

speak(ice_cream);
```

属性的顺序不重要。









