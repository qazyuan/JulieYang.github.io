---
title: The Execution Context
date: 2024-04-30 15:25:39
tags: 
  - js-base 
  - js
---

# 对执行上下文的理解 

### **Understanding the execution context** 

## 1. 执行上下文类型

**Execution context type** 

**（1）全局执行上下文**

**global execution context**

任何不在函数内部的都是全局执行上下文， 它首先会创建一个全局的window对象，并且设置this的值等于这个全局对象，一个程序中只有一个全局执行上下文。

Anything that is not inside a function is a global execution context. It first creates a global window object and sets the value of 'this' to be this global object. There is only one global execution context in a program.

**（2）函数执行上下文**

**function execution context**

当一个函数被调用时，就会为该函数创建一个新的执行上下文，函数的上下文可以有任意多个。

When a function is called, a new execution context is created for that function. There can be any number of function execution contexts.

**（3）**`**eval**`**函数执行上下文**

\* * eval * *  **function execution context**  

执行在eval函数中的代码会有属于他自己的执行上下文，不过eval函数不常使用，不做介绍。

The code executed in the eval function has its own execution context. However, the eval function is not commonly used and will not be discussed further.

## 2. 执行上下文栈

**Execute the context stack**

JavaScript引擎使用执行上下文栈来管理执行上下文
The JavaScript engine uses the execution context stack to manage the execution context.

当JavaScript执行代码时，首先遇到全局代码，会创建一个全局执行上下文并且压入执行栈中，每当遇到一个函数调用，就会为该函数创建一个新的执行上下文并压入栈顶，引擎会执行位于执行上下文栈顶的函数，当函数执行完成之后，执行上下文从栈中弹出，继续执行下一个上下文。当所有的代码都执行完毕之后，从栈中弹出全局执行上下文。
When JavaScript executes code, it first encounters the global code, creates a global execution context, and pushes it onto the execution stack. Whenever a function call is encountered, a new execution context is created for that function and pushed onto the top of the stack. The engine then executes the function at the top of the execution context stack. After the function has finished executing, the execution context is popped off the stack, and the engine continues to execute the next context. Once all the code has been executed, the global execution context is popped off the stack.

```javascript
let a = 'Hello World!';
function first() {
  console.log('Inside first function');
  second();
  console.log('Again inside first function');
}
function second() {
  console.log('Inside second function');
}
first();
//执行顺序
//先执行second(),在执行first()
```

## 3. 创建执行上下文

**Create an execution context** 

创建执行上下文有两个阶段：**创建阶段**和**执行阶段**

there are two phases to create an execution context: **creation phase** and **execution phase** 

**1）创建阶段** **creation phase** 

（1）this绑定 this binding 

​	在全局执行上下文中，this指向全局对象（window对象）
​	In the context of global execution, this points to the global object (window object).

​	在函数执行上下文中，this指向取决于函数如何调用。如果它被一个引用对象调用，那么 this 会被设置成那个对象，否则 this 的值被设置为全局对象或者 undefined
​	In the context of function execution, the value of this depends on how the function is called. If it is called by a reference object, this is set to that object; otherwise, this is set to the global object or undefined. 

（2）创建词法环境组件 create lexical environment components 

​	词法环境是一种有**标识符——变量映射**的数据结构，标识符是指变量/函数名，变量是对实际对象或原始数据的引用。
​	A lexical environment is a data structure that maps identifiers to variables. Identifiers refer to variable or function names, and variables are references to actual objects or primitive data.

​	环境记录器:用来储存变量个函数声明的实际位置**外部环境的引用**：可以访问父级作用域
​	Environment recorder: used to store the actual location of variable function declarations **references to external environments** : allow access to the parent scope. 

（3）创建变量环境组件 Create a variable environment component

​	变量环境也是一个词法环境，其环境记录器持有变量声明语句在执行上下文中创建的绑定关系。

​	The variable environment is also a lexical environment. Its Environment Record holds the bindings created by variable declarations within the execution context.

**2）执行阶段  execution phase**

此阶段会完成对变量的分配，最后执行完代码。

At this stage, variables are allocated and the code is executed. 



### **简单来说执行上下文就是指：**

**In short, the execution context refers:**



在执行JS代码之前，需要先解析代码。解析的时候会先创建一个全局执行上下文环境，先把代码中即将执行的变量、函数声明都拿出来，变量先赋值为undefined，函数先声明好可使用。这一步执行完了，才开始正式的执行程序。

Before executing JS code, it needs to be parsed first. During parsing, a global execution context is created. Variables and function declarations that will be executed in the code are extracted first: variables are initially assigned as undefined, and functions are declared and made available. After this step is completed, the formal execution of the program begins.

在一个函数执行之前，也会创建一个函数执行上下文环境，跟全局执行上下文类似，不过函数执行上下文会多出this、arguments和函数的参数。

Before a function is executed, a function execution context is also created, which is similar to the global execution context. However, the function execution context includes additional elements like this, arguments, and the function's parameters.

- 全局上下文：变量定义，函数声明
  global Context: variable definition, function declaration 
- 函数上下文：变量定义，函数声明，`this`，`arguments`
  function context: variable definition, function declaration, this , arguments
