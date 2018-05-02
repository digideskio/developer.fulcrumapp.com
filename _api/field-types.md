---
layout: default
section: api
order: 1
title: "Field Types"
description: "Field Types Overview - Sets, Choices, and Other Fields"
category: section
---

## Section

A section is a logical divider of the form. Sections may contains many form elements, and have sections nested beneath them.

* `Section` elements *must* contain at least 1 element.
* `Section` elements will be recursively validated using all of the rules described here.

## Classification Field

[Classification sets]({{ site.url }}/help/classification-sets/) let you predefine hierarchies and schemas for classifying data into a standard format from the field.

* `ClassificationField` elements *must* have a `classification_set_id` field that contains a **valid** `id` of an existing classification set object.

## Choice Field

A ChoiceField is a form element to allow a list of selectable options. There are two ways to define the options of a ChoiceField. You can define the choices inline with the ChoiceField, or you can use [Choice Lists]({{ site.url }}/endpoints/choice-lists/) to define a set of choices that can be shared across multiple forms. Both methods of defining the choices are supported by the API. See the API documentation for [Choice Lists]({{ site.url }}/endpoints/choice-lists/) for information on how to manage Choice Lists.

`ChoiceField` elements *must* have either:

- a `choices` array that contains a non-empty Array of Hashes of `{"label":"", "value":""}`
  - Every choice object within the `choices` array must contain at least a non-blank label
  - The `value` attribute of each choice is optional, and will default to the `label` if not given
- a `choice_list_id` field that contains a valid `id` of an existing ChoiceList object

If both the `choice_list_id` and a `choices` array are given, `choice_list_id` will be the object used in validation and on the mobile device. You may use `null` or not include it in the object. It's recommended to only supply one or the other in the form definition.

Attributes specific to a `ChoiceField`:
- `multiple` must be `true` or `false`. This determines if the mobile device can select a single or multiple choices in a ChoiceField.
- `allow_other` must be `true` or `false`. This allows a user to enter an additional choice of their own in the given choice field. This feature is supported for both single choice and multiple choice fields.

## Yes / No Field

Yes/No fields behave just like Single Choice fields, but present a different UI on the Fulcrum mobile client apps that allows you to quickly choose Yes, No or N/A without selecting from a list.

Attributes specific to `YesNoField`:

* `positive` - an object representing a positive choice having both `label` and `value` keys.
* `negative` - an object representing a negative choice having both `label` and `value` keys.
* `neutral` - an object representing a neutral choice having both `label` and `value` keys.
* `neutral_enabled` - a boolean that specifies if the neutral choice is enabled or not.

See a [JSON object of an example form](https://gist.github.com/JasonSanford/0c4c141282933a650b9c) that contains a valid YesNoField definition.

## Photo Field

This field allows photos to be captured using the mobile device. Each Photo field allows any number of photos to be added to it.

## Time Field

Time fields validate and store relative time (no timezone) with the time in 24-hour format like:

    13:30

## Conditionals

Every element in the form may have two types of conditionals: `required_conditions` (to force data entry into the given field) and `visible_conditions` (to hide/show fields dynamically -- to enable "decision tree"-like functionality). These allow for special conditions to be met before a field can be shown or a record can be created on the mobile device. These conditions are *not* used in the validation process for record upload.

Condition attributes are not required. They are only required if the type fields are specified as below:

* `required_conditions_type` and `visible_conditions_type` determine if there are conditions
* Both of the type fields, if provided, *must* be either `all` or `any` (equivalent of `AND` and `OR` to evaluate conditions)
* If 'any' or 'all' are in the `required_conditions_type` or `visible_conditions_type`, then:
  * `visible_conditions` and `required_conditions` must be an array of condition objects.
  * Each condition object contains a `field_key`, `operator`, and an optional `value` depending on the operator.
  * `field_key` MUST be a `key` of an existing element elsewhere on the form, it is the field being referenced
  * For ChoiceField's, `operator` MUST be one of the following:
    * `'equal_to', 'not_equal_to', 'is_empty', 'is_not_empty'`
  * For all other fields, `operator` can be any of the above, plus the following:
    * `'contains', 'starts_with', 'greater_than', 'less_than'`
  * `'is_empty'` and `'is_not_empty'` do not require a `value` attribute to be given

The example below shows the structure of a field with two visibility conditions. In this case, this "yes/no" selector field for capturing whether a venue has wifi or not is only displayed if the user selects "Diner" or "Coffee Shop" from another choice field earlier in the form:

```
{
  "allow_other": false,
  "choices": [
    {
      "label": "Yes",
      "value": "Y"
    },
    {
      "label": "No",
      "value": "N"
    }
  ],
  "data_name": "wifi",
  "default_value": null,
  "description": "Does the venue have wifi?",
  "disabled": false,
  "hidden": false,
  "key": "key",
  "label": "Wifi?",
  "multiple": false,
  "required": false,
  "required_conditions_type": null,
  "type": "ChoiceField",
  "visible_conditions": [
    {
      "field_key": "field_key",
      "operator": "equal_to",
      "value": "Diner"
    },
    {
      "field_key": "field_key",
      "operator": "equal_to",
      "value": "Coffee Shop"
    }
  ],
  "visible_conditions_type": "any"
}
```
