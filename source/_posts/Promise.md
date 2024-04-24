---
title: Promise
date: 2024-04-24 20:10:44
tags:
  - js
  - promise
---

## The Definition of Promises:

In JavaScript, a Promise is an object that represents the eventual completion (or failure) of an asynchronous operation and its resulting value. It is a proxy for a value that may not be known when the Promise is created. Promises allow you to associate handlers with an asynchronous action's eventual success value or failure reason. This enables asynchronous methods to return values similar to synchronous methods by returning a Promise to supply the value at some point in the future.

A Promise can be in one of three states:

1. **Pending**: The initial state when the Promise is neither fulfilled nor rejected.
2. **Fulfilled**: Indicates that the operation was completed successfully.
3. **Rejected**: Indicates that the operation failed.

When a Promise is settled, it means it is either fulfilled or rejected, but not pending. The term "resolved" is also used with Promises, indicating that the Promise is settled or "locked-in" to match the eventual state of another Promise. Once a Promise is resolved, further resolving or rejecting it has no effect.

Promises in JavaScript are commonly used for handling asynchronous operations in a more structured and manageable way. They provide a mechanism for handling the results of asynchronous tasks, allowing developers to write cleaner and more maintainable asynchronous code. Promises can be chained together using methods like `then()`, `catch()`, and `finally()` to handle the success and failure of asynchronous operations in a sequential manner.

## Promise.all() VS Promise.race()

Promise. all multiple Promise the instance is packaged as a new Promise instance. At the same time, the return values of success and failure are different. The return values of success are **an array of results** , and returns when it fails **the value of the first rejected failure state.** 

In Promise.all, an array is passed in, and an array is returned. The values returned by the input promise objects are arranged in the array in order. However, note that the order in which they are executed is not in order unless the iterable objects are empty. 

Note that the order of data in the array of successful results obtained by Promise.all is the same as that received by Promise.all. In this case, when multiple requests are sent and data is obtained and used according to the request order, Promise.all can be used to solve the problem.

**（2）Promise.race**
As the name implies, Promse.race means a race, which means that if any result in Promise.race([p1, p2, p3]) gets faster, the result is returned, regardless of whether the result itself is successful or failed. When you want to do something, how long will it take to stop doing it? You can use this method to solve it:

```js
Promise.race([promise1,timeOutPromise(5000)]).then(res=>{
  
})
```

## The Problem It Sloves

In my work, I often encounter such A requirement. For example, after I use ajax to send A request A, I get the data after success, and I need to pass the data to the request B; Then I need to write the following code:

```js
let fs = require('fs')
fs.readFile('./a.txt','utf8',function(err,data){
  fs.readFile(data,'utf8',function(err,data){
    fs.readFile(data,'utf8',function(err,data){
      console.log(data)
    })
  })
})
```

The preceding code has the following disadvantages:

- the latter request depends on the data passing down after the previous request is successful, which causes multiple ajax requests to be nested and the code is not intuitive enough. 
- if the first and second requests do not need to pass parameters, the next request needs to be executed after the previous request succeeds. In this case, the code needs to be written as above, resulting in insufficient intuitive code.

`Promise` after it appears, the code becomes as follows:

```js
let fs = require('fs')
function read(url){
  return new Promise((resolve,reject)=>{
    fs.readFile(url,'utf8',function(error,data){
      error && reject(error)
      resolve(data)
    })
  })
}
read('./a.txt').then(data=>{
  return read(data) 
}).then(data=>{
  return read(data)  
}).then(data=>{
  console.log(data)
})
```

In this way, the code looks much simpler and solves the problem of Hell callback.
