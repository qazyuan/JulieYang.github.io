---

title: Deep Copy
date: 2024-05-08 15:29:30
tags: 
  - js
  - object

---

**Deep copy VS Shallow copy**

**Shallow copy**: Shallow copy refers to copying the attribute value of one object to another object. If the attribute value is a reference type, then the address of the reference is copied to the object, so the two objects will have a reference of the same reference type. Shallow copying can be implemented using **Object.assign()** and the spread operator.

**Deep copy**: Compared to shallow copy, if a property value is a reference type, deep copy creates a reference type and copies the corresponding value to it, so the object gets a new reference type instead of a reference of the original type. Deep copying can be  achieved for some objects using two JSON functions. However, due to JSON's stricter object format compared to JavaScript's, this method fails if the object contains functions or Symbol types.

## **Implement deep copy**

### **（1）JSON.stringify()**

- **JSON.parse(JSON.stringify(obj))** is one of the most commonly used deep copy methods. Its principle is to use JSON.stringify to serialize the JavaScript object into a JSON string, and then use **JSON.parse()** to deserialize (restore) the JavaScript object.

- This method can be a simple implementation of deep copy, but there is a problem -  functions, undefined, and Symbol values in the copied object will be lost after processing with **JSON.stringify()**.

```javascript
let obj1 = {  a: 0,
              b: {
                 c: 0
                 }
            };
let obj2 = JSON.parse(JSON.stringify(obj1));
obj1.a = 1;
obj1.b.c = 1;
console.log(obj1); // {a: 1, b: {c: 1}}
console.log(obj2); // {a: 0, b: {c: 0}}
```

### **(2) Library lodash's.cloneDeep method**

The library also provides **_.cloneDeep()** for Deep Copying.

```javascript
var _ = require('lodash');
var obj1 = {
    a: 1,
    b: { f: { g: 1 } },
    c: [1, 2, 3]
};
var obj2 = _.cloneDeep(obj1);
console.log(obj1.b.f === obj2.b.f);// false
```

### **(3) Handwritten Deep Copy Function Implementation**

```javascript
// 深拷贝的实现
function deepCopy(object) {
  if (!object || typeof object !== "object") return;

  let newObject = Array.isArray(object) ? [] : {};

  for (let key in object) {
    if (object.hasOwnProperty(key)) {
      newObject[key] =
        typeof object[key] === "object" ? deepCopy(object[key]) : object[key];
    }
  }

  return newObject;
}
```
