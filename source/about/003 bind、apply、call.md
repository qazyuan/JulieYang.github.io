##  [day44] This

### 1. What does the code snippet to the right output by console.log?

```javascript
const obj = {
  a: 1,
  b: function() {
    console.log(this.a)
  },
  c() {
    console.log(this.a)
  },
  d: () => {
    console.log(this.a)
  },
  e: (function() {
    return () => {
      console.log(this.a);
    }
  })(),
  f: function() {
    return () => {
      console.log(this.a);
    }
  }
}

console.log(obj.a)
obj.b()
;(obj.b)()
const b = obj.b
b()
obj.b.apply({a: 2})
obj.c()
obj.d()
;(obj.d)()
obj.d.apply({a:2})
obj.e()
;(obj.e)()
obj.e.call({a:2})
obj.f()()
;(obj.f())()
obj.f().call({a:2})
```

A:

```javascript
console.log(obj.a) // 1
obj.b() // 1
;(obj.b)() // 1
const b = obj.b
b() // undefined
obj.b.apply({a: 2}) // 2
obj.c() // 1
obj.d() // undefined
;(obj.d)() // undefined
obj.d.apply({a:2}) // undefined
obj.e() // undefined
;(obj.e)() // undefined
obj.e.call({a:2}) // undefined
obj.f()() // 1
;(obj.f())() // 1
obj.f().call({a:2}) // 1
```

### 2. Can you explain 'this' in Javascript?

 Q: The this keyword in JavaScript refers to the context in which a piece of code, typically a function's body, is supposed to run. The value of this is dynamic and depends on how a function is invoked, not how it is defined. In JavaScript, this can have different values based on the context in which it is used.

1. When a regular function is invoked as a method of an object (e.g.,
   obj.method()), this points to that object.
2. When a regular function is invoked as a standalone function (not attached to an object), this typically refers to theglobal object (in non-strict mode) or is undefined (in strict mode). 
3. Arrow functions inherit this from the parent scope at the time they are defined, making them useful for callbacks and preserving context. However, arrow functions do not have their own this binding. 

The behavior of this can be further controlled using methods like call(), apply(), and bind(). These methods allow you to explicitly set the value of this for a particular function call. Arrow functions, on the other hand, retain the value of this from their enclosing lexical context.

In summary, this in JavaScript is a dynamic keyword that refers to the object
context in which a function is executed. Its value changes based on how a
function is called, providing flexibility in managing object contexts and
function invocations. Understanding how this works is crucial for effective
JavaScript programming.



## [day7] T**he execution context** 

### 1. How many types of the execution context in JavaScript?

Q: There are three types of execution context:

1. **global execution context**

   Anything that is not inside a function is a global execution context. It first creates a global window object and sets the value of 'this' to be this global object. There is only one global execution context in a program.

2. **function execution context**

   When a function is called, a new execution context is created for that function. There can be any number of function execution contexts.

3. `eval`**function execution context**  

   The code executed in the eval function has its own execution context. However, the eval function is not commonly used and will not be discussed further.

### 2. What is the execution context stack?

Q: The JavaScript engine uses the execution context stack to manage the execution context.

When JavaScript executes code, it first encounters the global code, creates a global execution context, and pushes it onto the execution stack. Whenever a function call is encountered, a new execution context is created for that function and pushed onto the top of the stack. The engine then executes the function at the top of the execution context stack. After the function has finished executing, the execution context is popped off the stack, and the engine continues to execute the next context. Once all the code has been executed, the global execution context is popped off the stack.

### 3. How many phases are there in creating an execution context and what does they do?

Q: there are two phases to create an execution context: **creation phase** and **execution phase**:

1. **creation phase** 

   1. this binding 

      In the context of global execution, this points to the global object (window object).

      In the context of function execution, the value of this depends on how the function is called. If it is called by a reference object, this is set to that object; otherwise, this is set to the global object or undefined. 

   2. create lexical environment components 

      A lexical environment is a data structure that maps identifiers to variables. Identifiers refer to variable or function names, and variables are references to actual objects or primitive data.

      Environment recorder: used to store the actual location of variable function declarations **references to external environments** : allow access to the parent scope. 

   3. Create a variable environment component

      The variable environment is also a lexical environment. Its Environment Record holds the bindings created by variable declarations within the execution context.

2. **execution phase**

   At this stage, variables are allocated and the code is executed. 

## [day4] arrow function

### 1. Can you explain the steps to implement the new operator?

Q: The steps to implement the new operator are as follows:

1. Create a new object
2. Assign the constructor function's scope to the new object (in other words, set the object's prototype property to point to the constructor function's prototype property)
3. Point to the code in the constructor, and this in the constructor points to the object (That is, to add properties and methods to the object)
4. Return a new object

### 2. Why we can't use `new` to create an arrow function?

Q: The arrow function is introduced in ES6. It does not have a prototype, nor does it have its own this reference, and it cannot use the arguments parameter. Therefore, it is not possible to use the new operator with an arrow function.

### 3. What ate the differences between arrow function and ordinary function?

Q: The difference between arrow functions and ordinary functions

**(1) Arrow functions are more concise than ordinary functions**

- If there are no arguments, just write an empty parenthesis
- If there is only one parameter, you can omit the parentheses for the parameter
- If there are multiple parameters, separate them with commas
- If the return value of the function body is only one sentence, you can omit the braces
- If the function body does not require a return value and only has one sentence, you can prefix the statement with a void keyword. The most common is to call a function:

```javascript
let fn = () => void doesNotReturn();
```

**(2) Arrow functions do not have their own `this` keyword.** 

The arrow function doesn't create its own this, so it doesn't have its own this, it just inherits this at the level above its own scope. So the pointer to this in the arrow function was already fixed when it was defined, and it doesn't change after that.

**（3）This pointer inherited from the arrow function never changes**

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

Method b of object obj is defined using an arrow function, and this in this function always points to this in the global execution environment in which it was defined, even if the function is called as a method of object obj, this still points to the Window object.

It is worth noting that the curly brackets {} used to define objects cannot form a separate execution environment, they still remain in the global execution environment.

**（4）call()、apply()、bind() etc. cannot change the reference of 'this' in arrow functions.**

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

**（5）Arrow functions cannot be used as constructors**

The process of constructing a function with 'new' has been explained above. In fact, the second step is to refer the 'this' in the function to the object. However, since arrow functions do not have their own 'this' and the 'this' refers to the outer execution environment, and cannot be changed, they cannot be used as constructors.

**（6）Arrow functions do not have their own arguments.**

The arrow function does not have its own arguments object. Accessing arguments in an arrow function actually gets the arguments value of its outer function.

**（7）Arrow functions do not have a prototype.**

**（8）Arrow functions cannot be used as Generator functions and cannot use the yeild keyword **



