---
layout: default
section: expressions
title: "EXACT"
description: "Tests whether two strings are identical."
category: section
permalink: /expressions/reference/exact/
---

### Parameters

`value` String (__required__) - First string to compare.

`value` String (__required__) - Second string to compare.

### Returns

Boolean

### Examples

```js
EXACT("String 1", "String 1")

// returns true
```


```js
EXACT("String 1", "Another String")

// returns false
```


```js
EXACT(4,4)

// returns true
```