---
title: Arrow function
date: 2024-04-30 15:44:52
tags:
  - js
  - js-base
---

## 1.如果创建一个新的箭头函数会怎么样？

What happens if you create a new arrow function

箭头函数是ES6中的提出来的，它没有prototype，也没有自己的this指向，更不可以使用arguments参数，所以不能New一个箭头函数。

The arrow function is introduced in ES6. It does not have a prototype, nor does it have its own this reference, and it cannot use the arguments parameter. Therefore, it is not possible to use the new operator with an arrow function.

### new操作符的实现步骤如下：

The steps to implement the new operator are as follows:

1. 创建一个对象    

​	   	Create a new object

2. 将构造函数的作用域赋给新对象（也就是将对象的**proto**属性指向构造函数的prototype属性）

​		Assign the constructor function's scope to the new object (in other words, set the object's prototype property to point to the constructor function's prototype property)

3. 指向构造函数中的代码，构造函数中的this指向该对象（也就是为这个对象添加属性和方法）

​		Point to the code in the constructor, and this in the constructor points to the object (That is, to add properties and methods to the object)

4. 返回新的对象 

​		Return a new object

所以，上面的第二、三步，箭头函数都是没有办法执行的。

Therefore, the arrow functions in the above steps cannot be executed.

## 2.箭头函数与普通函数的区别

The difference between arrow functions and ordinary functions

**（1）箭头函数比普通函数更加简洁**

​	**Arrow functions are more concise than ordinary functions**

- 如果没有参数，就直接写一个空括号即可
- If there are no arguments, just write an empty parenthesis
- 如果只有一个参数，可以省去参数的括号
- If there is only one parameter, you can omit the parentheses for the parameter
- 如果有多个参数，用逗号分割
- If there are multiple parameters, separate them with commas
- 如果函数体的返回值只有一句，可以省略大括号
- If the return value of the function body is only one sentence, you can omit the braces
- 如果函数体不需要返回值，且只有一句话，可以给这个语句前面加一个void关键字。最常见的就是调用一个函数：
- If the function body does not require a return value and only has one sentence, you can prefix the statement with a void keyword. The most common is to call a function:

```javascript
let fn = () => void doesNotReturn();
```

**（2）箭头函数没有自己的this**

​	Arrow functions do not have their own 'this' keyword. 

​	箭头函数不会创建自己的this， 所以它没有自己的this，它只会在自己作用域的上一层继承this。所以箭头函数中this的指向在它在定义时已经确定了，之后不会改变。

​	The arrow function doesn't create its own this, so it doesn't have its own this, it just inherits this at the level above its own scope. So the pointer to this in the arrow function was already fixed when it was defined, and it doesn't change after that.

**（3）箭头函数继承来的this指向永远不会改变**

​	This pointer inherited from the arrow function never changes

```javascript
var id = 'GLOBAL';
var obj = {
  id: 'OBJ',
  a: function(){
    console.log(this.id);
  },
  b: () => {
    console.log(this.id);
  }
};
obj.a();    // 'OBJ'
obj.b();    // 'GLOBAL'
new obj.a()  // undefined
new obj.b()  // Uncaught TypeError: obj.b is not a constructor
```

​	对象obj的方法b是使用箭头函数定义的，这个函数中的this就永远指向它定义时所处的全局执行环境中的this，即便这个函数是作为对象obj的方法调用，this依旧指向Window对象。

​	Method b of object obj is defined using an arrow function, and this in this function always points to this in the global execution environment in which it was defined, even if the function is called as a method of object obj, this still points to the Window object.

​	需要注意，定义对象的大括号`{}`是无法形成一个单独的执行环境的，它依旧是处于全局执行环境中。

​	It is worth noting that the curly brackets {} used to define objects cannot form a separate execution environment, they still remain in the global execution environment.

**（4）call()、apply()、bind()等方法不能改变箭头函数中this的指向**

The methods call(), apply(), bind(), etc. cannot change the reference of 'this' in arrow functions.

```javascript
var id = 'Global';
let fun1 = () => {
    console.log(this.id)
};
fun1();                     // 'Global'
fun1.call({id: 'Obj'});     // 'Global'
fun1.apply({id: 'Obj'});    // 'Global'
fun1.bind({id: 'Obj'})();   // 'Global'
```

**（5）箭头函数不能作为构造函数使用**

​	**Arrow functions cannot be used as constructors**

​	构造函数在new的步骤在上面已经说过了，实际上第二步就是将函数中的this指向该对象。 但是由于箭头函数时没有自己的this的，且this指向外层的执行环境，且不能改变指向，所以不能当做构造函数使用。

​	The process of constructing a function with 'new' has been explained above. In fact, the second step is to refer the 'this' in the function to the object. However, since arrow functions do not have their own 'this' and the 'this' refers to the outer execution environment, and cannot be changed, they cannot be used as constructors.

**（6）箭头函数没有自己的arguments**

​	Arrow functions do not have their own arguments.

​	箭头函数没有自己的arguments对象。在箭头函数中访问arguments实际上获得的是它外层函数的arguments值。

​	The arrow function does not have its own arguments object. Accessing arguments in an arrow function actually gets the arguments value of its outer function.

**（7）箭头函数没有prototype**

​	Arrow functions do not have a prototype.

**（8）箭头函数不能用作Generator函数，不能使用yeild关键字**

​	Arrow functions cannot be used as Generator functions and cannot use the yeild keyword
