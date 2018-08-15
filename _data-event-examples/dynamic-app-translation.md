---
layout: default
section: data_events
title: "Dynamically translate form elements"
description: "Demonstrates how to dynamically translate labels and choice list options to support multiple languages."
category: section
tags:
  - set label
  - set description
  - set choices
---

This example demonstrates how to use the [SETLABEL](/data-events/reference/setlabel/) and [SETCHOICES](/data-events/reference/setchoices/) functions to dynamically update the form elements when a user selects their language from a choice list. The translations are stored as simple JavaScript objects. Note that in addition to labels and choices, you can also use this method to translate field descriptions with [SETDESCRIPTION](/data-events/reference/setdescription/).

<img src="../media/form-translation.gif" alt="Socrata Example" style="float: right; margin-left: 40px;" />

```js
var labels = {
  'name': {
    'Spanish': 'Nombre',
    'French': 'Nom'
  },
  'age': {
    'Spanish': 'Años',
    'French': 'Âge'
  },
  'gender': {
    'Spanish': 'Género',
    'French': 'Sexe'
  }
};

var choices = {
  'gender': {
    'Spanish': [
      {
        'label': 'Varón',
        'value': 'Male'
      },
      {
        'label': 'Hembra',
        'value': 'Female'
      }
    ],
    'French': [
      {
        'label': 'Mâle',
        'value': 'Male'
      },
      {
        'label': 'Femelle',
        'value': 'Female'
      }
    ]
  }
};

function translate(e) {
  var language = CHOICEVALUE($language);
  DATANAMES().forEach(function(dataName) {
    // Update field labels
    if (labels[dataName]) {
      SETLABEL(dataName, labels[dataName][language]);
    } else {
      SETLABEL(dataName, null);
    }
    // Update choice values
    if (choices[dataName] && choices[dataName][language]) {
      SETCHOICES(dataName, choices[dataName][language]);
    } else {
      SETCHOICES(dataName, null);
    }
  });
}

ON('load-record', translate);
ON('change', 'language', translate);
```
