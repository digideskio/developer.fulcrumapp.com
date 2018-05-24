---
layout: default
section: expressions
title: "DATANAMES"
description: "Returns the data names of the form fields"
category: section
permalink: /expressions/reference/datanames/
---

### Parameters

`type` String (optional)  [default = any] - Optional field type

### Returns

Array

### Examples

```js
DATANAMES()

// returns ["name","items","cost","choice_value","child_items","child_item_cost","choice_field"]
```


```js
DATANAMES('Repeatable')

// returns ["items","child_items"]
```