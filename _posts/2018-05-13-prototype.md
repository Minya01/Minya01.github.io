---
layout: post
title:  简述ES6/ES5原型链与继承
date:   2018-05-13
categories: prototype
---

ES6之前，JavaScript没有类的概念，继承基于原型链。每个对象都有一个指向另一个对象或内部的[[prototype]]属性。



### prototype

prototype属性使你有能力向对象添加属性和方法。例如：

```javascript
object.prototype.name = value;
```



### 原型链

每个人对象都有一个指向它原型（prototype）对象的内部链接。每个原型对象又有自己的原型，知道某个对象的原型为null为止，这条链则终止。



##### 原型链继承

将父类的实例作为子类的原型。

```javascript
function Person(){
    this.name = 'summer';
}

Person.prototype.getName = function(){
    return this.name;
}

function Girl(){
    
}

// 继承Person  (原型链继承)
Girl.prototype = new Person();

// 创建girl1
var girl1 = new Girl();
console.log(girl1.getName()) //summer
```



##### 构造函数继承

使用父类的构造函数来增强子类实例（复制父类的实例属性给子类）

```javascript
...
function Girl(){
    Person(this);//实现继承
    // this.name = 'minya';
}

var girl2 = new Girl();
console.log(girl2.name); // summer
```

在ES5中，所有的构造函数的`__proto__`都指向Function.prototype。

在ES6中，构造函数的`__proto__`指向它的父类构造函数。

如：

```javascript
object.__proto__ === object.[[prototype]];
// ES5
girl1.__proto___ === Girl.prototype;
// ES6
girl1.__proto__ === Girl
```



##### ES6继承

ES6提供了基础类Class。

例如：

```javascript
class Person{
    constructor(name,age) {
        this.name = name;
        this.age = age;
    }
    sayHello() {
        console.log(`My name is ${this.name}, I'm ${this.age} years old.`)
    }
    toString() {
        return `My name is ${this.name}, I'm ${this.age} years old.`;
    }
}

class Girl extends Person {
    constructor(name,age,sex) {
        super(name,age);//继承父类的contructor
        this.sex = sex;
    }
    aboutSex() {
        console.log(`I'm a ${this.sex}.`)
    }
    toString() {
        return this.aboutSex() + super.toString();// 调用父类的toString()
    }
}
```

> 注意，子类必须在constructor中先使用super()调用父类
>
> 因为子类通过super获取父类this实现继承，否则后边的this.sex会因获取不到this而报错。
>
> super关键字，有两种用法：
>
> > * 作为函数调用（super(...args)），super代表父类的构造函数。
> > * 作为对象调用（super.prop或super.method()），super代表父类。注，此时super可以引用父类实例的属性和方法，也可以引用父类的静态方法。



###### 静态方法

类似于原型方法，但是不能由类实例调用。

```javascript
class Person{
    ...
    
    static personExamples() {
        // 静态方法
        return `这是个静态方法，不能由类实例调用，可以用super调用`
    }
}

class Girl extends Person {
    ...
    
    showTime() {
        console.log(
        	Person.personExamples(),
            super.personExamples()
        );
    }
}
```



关于继承，还有很多需要补充的点，待完善。











