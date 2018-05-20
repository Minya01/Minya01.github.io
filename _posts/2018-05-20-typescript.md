---
layout: post
title:  typescript学习
date:   2018-05-21
categories: typescript
---

出差杭州一周，36度的大杭州真是热啊，这两天下雨一下子就降了12度，逛了西湖，烟雨蒙蒙，忍不住就哼唱一句“西湖的美，我的泪....”

废话不多说，收收心，继续努力工作。



TypeScript是一种强类型，面向对象的编译语言。TypeScript既是语言，也是工具。TypeScript是JavaScript类型的超集，可以编译成JavaScript。



##### 为什么使用TypeScript

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

属性的顺序不重要。只需要属性出现并类型正确，否则会报错。



##### 类

TypeScript提供了与Java类似的面向对象的编码风格，包括继承、抽象类、接口实现、setter/getter等。

目前ES6也已经支持类，只是TypeScript更严格。

如：

```typescript
class Menu {
    // 属性
    // 默认情况下是公共的
    items: Array<string>;
    pages: number;
    
    // 构造函数
    constructor(item_list: Array<string>, total_pages: number){
        this.items = item_list;
        this.pages = total_pages;
    }
    
    // 方法
    list():void {
        console.log('menu:')
        for(var i = 0;i < this.items.length; i++){
            console.log(this.items[i]);
        }
    }
}

//实例
var menu1 = new Menu(['book','food','package'],1);
menu1.list();
```

继承：

```typescript
class HappyMenu extend Menu {
    // 属性被继承
    
    // 重新定义一个新的构造函数
    constructor(item_list: Array<string>, total_pages: number){
        super(item_list,total_pages);
    }
    
    // 方法
    list():void {
        ...
    }
}
    
 // 实例
 var happy1 = new HappyMenu(['summer','minya','toy'],1);
 happy1.list();  
```



##### 泛型

泛型允许相同函数接受不同类型参数的模板。使用泛型创建可重用组件比使用```any```数据类型好，因为泛型保留了变量的类型。

例如：

```typescript
// 函数名称后的<T>表示它是一个通用函数
// 当调用时，T的每个实例都将被实际提供的类型替换

function genFunc<T>(argument: T): T[]{
    var arrayOfT : T[] = [];
    arrayOfT.push(argument);
    return arrayOfT;
}

var arrayFromString = genFunc<string>('summer');
console.log(arrayFromString[0]);// summer
console.log(typeof arrayFromString[0]);//string
```



##### 模块

TypeScript引入了用于导出和导入模块的语法，但无法处理文件质检的实际连线。要启用外部模块，TS依赖于第三方库，比如浏览器应用程序的require.js或用于Node.js的CommonJS。

例如，两个文件，一个导出一个函数，一个导入并调用它。

export.ts

```typescript
var hello = function(): void {
    console.log('hello');
}
export = hello;
```

import.ts

```typescript
import hello = require('./export');
hello();
```

编译运行需要添加额外参数告知正在使用require.js

```ruby
tsc --module amd * .ts
```



以上是TypeScript学习一个阶段的理论小结。









