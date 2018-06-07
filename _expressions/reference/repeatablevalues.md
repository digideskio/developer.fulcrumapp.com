---
layout: default
section: expressions
title: "REPEATABLEVALUES"
description: "Return a specific field from multiple repeatable items"
category: section
permalink: /expressions/reference/repeatablevalues/
---

### Parameters

`repeatableVariable` Object (__required__) - The repeatable field variable

`dataName` String (__required__) - The data name of the field to extract from each repeatable item

### Returns

Array - An array of values of the `dataName` field from each item

### Examples

```js
REPEATABLEVALUES($repeatable_field, 'cost')

// returns [1,2,3]
```


```js
REPEATABLEVALUES($repeatable_field, 'choice_value').map(CHOICEVALUE)

// returns ["widget","spinner","gizmo"]
```

```js
REPEATABLEVALUES($repeatable_field, ['child_repeatable', 'grandchild_repeatable', 'field_inside_nested_rep']);

//returns the values in the field_inside_nested_rep fields in the grandchild_repeatable section.
//
//level of calculation field
// * repeatable_field
//  * child_repeatable
//   * grandchild_repeatable
//    * field_inside_nested_rep
```
