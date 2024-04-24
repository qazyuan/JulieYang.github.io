---
title: decode-message
date: 2024-04-07 16:19:12
tags:
  - js
  - array
  - algorithm
---

## Problem Description

Your are given a 2-D array of characters. There is a hidden message in it.

```
I B C A L K A
D R F C A E A
G H O E L A D
```

The way to collect the message is as follows:

1. start at top left
2. move diagonally down right
3. when cannot move any more, try to switch to diagonally up right
4. when cannot move any more, try switch to diagonally down right, repeat 3 stop
   when cannot neither move down right or up right. the character on the path is
   the message for the input above, IROCLED should be returned.

**notes** if no characters could be collected, return empty string

## The Solution

We'll create a function called `decode` that accepts a 2D array and decodes the
message using the following approach:

1. If the length of the array or message is 0, return an empty string.
2. Loop through the columns from 0 to the last column.
3. Keep track of the coordinates and the direction (default is positive, meaning
   going down).
4. Use a while loop to loop through from column 0 to the last column and collect
   the result.
5. When you match the end, switch the direction.

## Edge Cases

- If the length of the array or message is 0, return an empty string.
- If the row is bigger than the rows or smaller than 0, change the direction.
- If there's only one row, handle the edge case to avoid overflow.

## The slove

```
/**
 * @param {string[][]} message
 * @return {string}
 */
function decode(message) {
  if(!message.length) return "";
  if(!message[0].length) return "";

  const rows = message.length;
  const cols = message[0].length;

  let result = ''
  let row = 0
  let col = 0
  let directionY = 1
  while(col < cols && row > -1 && row < rows) {
    result += message[row][col]
    col += 1
    row = row + directionY

    if (row > rows - 1) {
      directionY = -1
      row -= 2
    } else if (row < 0) {
      directionY = 1
      row +=2
    }
  }
  return result
}


```

> 该题目来自https://bigfrontend.dev/problem/decode-message
