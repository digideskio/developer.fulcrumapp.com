---
layout: default
section: expressions
title: "STRING"
description: "Converts the given parameter to a string value"
category: section
permalink: /expressions/reference/string/
---

### Parameters

`anything` Object (__required__) - Any value

### Returns

String

### Examples

```js
STRING(1, 2, 3)

// returns "1, 2, 3"
```


```js
STRING($choice_field)

// returns "Red, Green, Blue"
```


```js
STRING(1)

// returns "1"
```


```js
STRING(true)

// returns "true"
```


```js
STRING([1, 2, 3])

// returns "1, 2, 3"
```


```js
STRING(null)

// returns ""
```