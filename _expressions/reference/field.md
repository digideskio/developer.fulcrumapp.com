---
layout: default
section: expressions
title: "FIELD"
description: "Returns definition object for a specified field"
category: section
permalink: /expressions/reference/field/
---

### Parameters

`dataName` String (__required__) - The data name of the field

### Returns

Object

### Examples

```js
FIELD('child_item_cost').label

// returns "Child Item Cost"
```


```js
FIELD('child_item_cost').parent.label

// returns "Child Items"
```