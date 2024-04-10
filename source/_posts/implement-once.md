---
title: implement-once
date: 2024-04-10 15:58:39
tags:
---

## Problem Description

\_.once(func) is used to force a function to be called only once, later calls
only returns the result of first call.

Can you implement your own once()?

```
function func(num) {
  return num
}

const onced = once(func)

onced(1)
// 1, func called with 1

onced(2)
// 1, even 2 is passed, previous result is returned
```

## The Solution

load-once is a technique used to ensure that a function is called only once.
Subsequent calls will return the result of the first call.

So, we nedd to record whether the function is called and the result. It is an
easy question. Let's do it.

## The resolve

```
function once(func) {
    let result = null;
    let isCalled = false;

    return function(...args) {
        if(isCalled) {
            return result;
        }

        result = func.call(this, ...args);
        isCalled = true;
        return result
    }

}
```

> 该题目来自https://bigfrontend.dev/problem/implement-once
