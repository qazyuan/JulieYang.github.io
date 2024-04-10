---
title: find-the-largest-difference
date: 2024-04-10 15:40:04
tags:
---

## Problem Description

Given an array of numbers, pick any two numbers a and b, we could get the
difference by Math.abs(a - b).

Can you write a function to get the largest difference?

```
largestDiff([-1, 2,3,10, 9])
// 11, obviously Math.abs(-1 - 10) is the largest

largestDiff([])
// 0

largestDiff([1])
// 0
```

## The Solution

we're given an array of numbers. The task is to pick any two numbers, find the
difference, and determine the largest difference. Here's how we can tackle this
problem:

First, let's write a function to calculate the largest difference. We'll need to
handle an edge case: if the length of the array is zero, return 0.

Next, we can calculate the difference between the maximum and minimum values. In
just two lines of code, we can accomplish this with:

```
    if (array.length == 0) {
        return 0
    } else {
        return max(array) - min(array)
    }
```

Second， if we are not allowed to use Math.min() and Math.max(), we can use loop
to find the max and min and then get the result.

To do this, we'll keep track of the minimum with a value of infinity, and set
the initial value for max to negative infinity.

The we use the loop function to compare the number one by one and calculate the
result.

## The resolve

```
function largestDiff(arr) {
  if (!arr.length) {
    return 0;
  }
  let min = Infinity;
  let max = - Infinity;
  let result = - Infinity;

  for (let item in arr) {
    if (item < min) {
      min = item;
      result = max - min;
    }
    if (item > max) {
      max = item;
      result = max -min;
    }
  }

  return result;
}
```

> 该题目来自https://bigfrontend.dev/problem/Find-the-largest-difference
