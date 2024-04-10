---
title: promisify
date: 2024-04-10 16:24:31
tags:
---

## Problem Description

Let's take a look at following error-first callback.

```
const callback = (error, data) => {
  if (error) {
    // handle the error
  } else {
    // handle the data
  }
}
```

Now think about async functions that takes above error-first callback as last
argument.

```
const func = (arg1, arg2, callback) => {
  // some async logic
  if (hasError) {
    callback(someError)
  } else {
    callback(null, someData)
  }
}
```

You see what needs to be done now. Please implement promisify() to make the code
better.

```
const promisedFunc = promisify(func)

promisedFunc().then((data) => {
  // handles data
}).catch((error) => {
  // handles error
})

```

## The Solution

Our goal is to convert this callback-based function into a promise-based one.

The promisify function converts the error-first callback pattern to a
promise-based API. It returns a new function that produces a promise. When the
promise is resolved, we get the data. If it's rejected, it has the error.

## The resolve

```
/**
 * @param {(...args) => void} func
 * @returns {(...args) => Promise<any}
 */
function promisify(func) {
  return function(...args) {
    return new Promise((resolve, reject) =>{
    func.call(this, ...args, (error, data) => {
      if(error) {
        reject(error)
      } else {
        resolve(data)
      }
    })
  })
  }
}
```

> 该题目来自https://bigfrontend.dev/problem/promisify
