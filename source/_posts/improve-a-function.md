---
title: improve-a-function
date: 2024-04-10 10:41:07
tags:
  - js
  - algorithm
---

## Problem Description

```
// Given an input of array,
// which is made of items with >= 3 properties

let items = [
  {color: 'red', type: 'tv', age: 18},
  {color: 'silver', type: 'phone', age: 20},
  {color: 'blue', type: 'book', age: 17}
]

// an exclude array made of key value pair
const excludes = [
  {k: 'color', v: 'silver'},
  {k: 'type', v: 'tv'},
  ...
]

function excludeItems(items, excludes) {
  excludes.forEach( pair => {
    items = items.filter(item => item[pair.k] === item[pair.v])
  })

  return items
}
```

1. What does this function excludeItems do?

2. Is this function working as expected ?

3. What is the time complexity of this function?

4. How would you optimize it ?

## The Solution

Our task is to filter the items based on the conditions specified in the
"exclude" array. For example, if the conditions are {"color": "silver"} and
{"type": "TV"}, then the item that matches both conditions (i.e., a silver TV)
would be excluded.

But, as we'll see, there's a typo in the function that prevents it from working
as expected.

The first thing we need to do is fix the typo in the function. Instead of
excluding items that match the conditions, the function is currently excluding
items that don't match the conditions. To fix this, we need to change the `!==`
operator to `===` in the if statement. Also the condition should be
`item[pair.k] !== pair.v`

The optimized version of the excludeItems function still has a time complexity
of O(n \* m), where n is the number of items in the items array and m is the
number of key-value pairs in the excludes array. However, the optimization lies
in the fact that it only iterates over the items array once, regardless of the
number of key-value pairs in the excludes array.

The optimization involves creating a set of excluded properties and values
first, and then filtering the items array based on this set. By doing so, it
avoids iterating over the entire items array multiple times, which was the case
in the original implementation.

The optimized version achieves better performance by iterating over the items
array only once and using a set to efficiently check for excluded properties and
values. This approach reduces the number of iterations and comparisons needed to
filter the items, leading to improved efficiency compared to the original
implementation.

## The resolve

```
function excludeItems(items, exclude) {
  const excludeMap = new Map();
  for (const condition of exclude) {
    const {key, value} = condition;
    if (!excludeMap.has(key)) {
      excludeMap.set(key, new Set());
    }
    excludeMap.get(key).add(value);
  }

  return items.filter(item => {
    for (const [key, valueSet] of excludeMap.entries()) {
      if (valueSet.has(item[key])) {
        return false;
      }
    }
    return true;
  });
}
```

> 该题目来自https://bigfrontend.dev/problem/improve-a-function
