---
title: Traverse-DOM-level-by-level
date: 2024-04-10 15:22:57
tags:
  - js
  - tree
  - algorithm
---

## Problem Description

Given a DOM tree, flatten it into an one dimensional array, in the order of
layer by layer, like below.
![](https://cdn.bfe.dev/bfe/img/ykqFdOIOaXFyn2uZ8h5Lt02sFaYb5eZ8_1063x546_1598232821941.png)

## The Solution

We need to traverse it level by level, resulting in this array:  
`[div, p, p, div, a, img, button]`

This problem is a typical BFS (Breadth-First Search) problem, which we can solve
using a queue. Here's a step-by-step approach:

1. Create a queue and add the root to it.

2. While the queue is not empty, shift the head out and add it to the result.

3. Add the children of the current node to the queue.

4. Repeat steps 2-3 until the queue is empty.

## The resolve

```
function traverseDOM(root) {
  if (!root) return [];

  const result = [];
  const queue = [root];

  while (queue.length > 0) {
    const current = queue.shift();
    result.push(current);

    if (current.children) {
      for (let child of current.children) {
        queue.push(child);
      }
    }
  }

  return result;
}
```

> 该题目来自https://bigfrontend.dev/problem/Traverse-DOM-level-by-level
