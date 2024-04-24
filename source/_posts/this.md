---
title: this
date: 2024-04-09 15:44:19
tags:
  - js
  - this
---

## Problem Description

What does the code snippet to the output by `console.log`

```
const obj = {
	a: 1,
	b: function () {
		console.log(this.a);
	},
	c() {
		console.log(this.a);
	},
	d: () => {
		console.log(this.a);
	},
	e: (function () {
		return () => {
			console.log(this.a);
		};
	})(),
	f: function () {
		return () => {
			console.log(this.a);
		};
	},
};

	console.log(obj.a);
	obj.b();
	;(obj.b)();

	const b = obj.b;
	b();
	obj.b.apply({ a: 2 });
	obj.c();
	obj.d();
	;(obj.d)();
	obj.d.apply({ a: 2 });
	obj.e();
	;(obj.e)();
	obj.e.call({ a: 2 });
	obj.f()();
	;(obj.f())();
	obj.f().call({ a: 2 });
```

## The Solution

- `const b = obj.b; b(); // undefined`

  lose this binding

- `obj.b.apply({ a: 2 }); `

  this will be altered into `{a:2}`, note that the origin `obj.a` will not be
  altered.

- `obj.c(); //1`

- `obj.d(); // undefined`

  arrow function, this is actually window

- `;(obj.d)(); // undefined`

- `obj.d.apply({ a: 2 }); // undefined`

  This behavior occurs because `obj.d` is an arrow function. Arrow functions do
  not bind their own this context but inherit it from the surrounding lexical
  context where they were defined. In this case, since `obj.d` is defined within
  the object `obj`, the arrow function `d` retains the this value from the
  surrounding context, which is the global object.

- `obj.e();` `;(obj.e)();` `obj.e.call({ a: 2 });`

  In the case of `obj.e`, the arrow function is created inside _ an immediately
  invoked function expression (IIFE) _. Arrow functions inherit this from the
  surrounding lexical context where they are defined. In this scenario, the
  arrow function inside `obj.e` will capture the this value of the surrounding
  context, which is the global object (typically window in a browser
  environment).

  Therefore, when `obj.e` is invoked, the this keyword inside the arrow function
  will refer to the global object, maintaining the same this reference as the
  surrounding context where the arrow function was defined.

  In summary, when executing `obj.e`, the this keyword inside the arrow function
  defined within `obj.e` will point to the global object (window in a browser
  environment) due to the lexical scoping behavior of arrow functions.
  immediately function, return the arrow function just as the `d()`, so this is
  also points to the Window

- `obj.f()(); ` `;(obj.f())();` `obj.f().call({ a: 2 });`

  When invoking `obj.f()()`, the this keyword inside the arrow function returned
  by `obj.f() ` will refer to the object `obj`. This behavior occurs because the
  arrow function inherits its this value from the surrounding lexical context
  where it was defined, which is the object `obj` in this case.

  Therefore, when `obj.f()()` is executed, the this keyword inside the arrow
  function will maintain its reference to the object `obj`, as it inherits the
  this value from the lexical scope in which it was created.

  In summary, when calling `obj.f()()`, the this keyword inside the arrow
  function returned by `obj.f()` will point to the object obj.

## The result

```javascript
// 1
console.log(obj.a);

//1
obj.b();

//1
obj.b();

// undefined
const b = obj.b;
b();

// 2
obj.b.apply({ a: 2 });

// 1
obj.c();

// undefined
obj.d();
obj.d();
obj.d.apply({ a: 2 });

// undefined
obj.e();
obj.e();
obj.e.call({ a: 2 });

// 1
obj.f()();
// 1
obj.f()();

// 1
obj.f().call({ a: 2 });
```

> 该题目来自https://bigfrontend.dev/quiz/this
