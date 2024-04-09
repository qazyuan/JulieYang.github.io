---
title: detect-data-type
date: 2024-04-07 17:44:30
tags:
---

## Problem Description

This is an easy problem.

For [all the basic data types](https://javascript.info/types "null") in JavaScript, how could you write a function to detect the type of arbitrary data?

Besides basic types, you need to also handle also commonly used complex data type including `Array`, `ArrayBuffer`, `Map`, `Set`, `Date` and `Function`

The goal is not to list up all the data types but to show us how to solve the problem when we need to.

The type should be lowercase

```js
detectType(1); // 'number'
detectType(new Map()); // 'map'
detectType([]); // 'array'
detectType(null); // 'null'

// more in judging step
```

## The Solution

We'll use object prototype and regular expressions:

1. Use `Object.prototype.toString.call(data).toLowerCase()` to get the object type as a string, such as [object string]
2. Use the regular expressions to extract the type from the string.
3. Create a Set of allowed types [all the basic data types] and check if the extracted type is in the set
4. Use a while loop to loop through from column 0 to the last column and collect the result.
5. if the type is in the set, we return it. Or we return 'object'

## The slove

```javascript
/**
 * @param {any} data
 * @return {string}
 */
function detectType(data) {
	const tag = Object.prototype.toString.call(data).toLowerCase();
	const matches = tag.match(/^\[object\s(.*?)\]/);
	if (matches) {
		const type = matches[1];
		console.log(type);
		const allowedTypes = new Set([
			"number",
			"bigint",
			"null",
			"string",
			"boolean",
			"symbol",
			"function",
			"undefined",
			"array",
			"date",
			"map",
			"set",
			"arraybuffer",
		]);
		if (allowedTypes.has(type)) {
			return type;
		}
	}
	return "object";
}
```

> 该题目来自https://bigfrontend.dev/problem/decode-message
