---
layout: default
section: endpoints
order: 5
title: "Choice Lists"
description: "Create, read, update, or delete choice lists"
category: section
img: /assets/img/guides/
difficulty: advanced
menu:
  - "Endpoints": endpoints
  - "Query Parameters": query-parameters
  - "Properties": choice-list-properties
  - "Validations": validations
  - "Notes": notes
  - "Examples": examples
---

The Choice Lists API gives you access to the [choice lists](http://help.fulcrumapp.com/apps/how-do-choice-lists-work) within your Fulcrum account. Choice lists can be shared between multiple apps.

## Endpoints

{:.table.table-striped}
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/v2/choice_lists.json | Fetch all choice lists. |
| GET | /api/v2/choice_lists/**:id**.json | Fetch a single choice list. |
| POST | /api/v2/choice_lists.json | Create a new choice list. |
| PUT | /api/v2/choice_lists/**:id**.json | Update a single choice list. |
| DELETE | /api/v2/choice_lists/**:id**.json | Delete a single choice list. |

## Query Parameters

{:.table.table-striped}
| Parameter | Type | Description |
|-----------|------|-------------|
| page | integer | The page number requested. |
| per_page | integer | Number of records per page. By default, all requests are paginated to the maximum value of 20,000 items per request. |

## Choice List Properties

{:.table.table-striped}
| Property | Type | Required | Readonly | Description |
|----------|------|----------|----------|-------------|
| name | string | yes | no | The name of the choice list. |
| choices | array of choice objects | yes | no | The choice list options. (`[{"label": "Excellent Condition","value": "excellent"},{"label": "Poor Condition","value": "poor"}`) |
| description | string | no | no | Optional choice list description. |
| version | number | no | yes | Autoincremented version of the choice list for history tracking. |
| id | string | no | yes | The id of the choice list. |
| created_at | string | no | yes | Timestamp when the choice list was created. |
| updated_at | string | no | yes | Timestamp when the choice list was last updated. |

## Validations

The following properties must be included in order to create/update a choice_list object in our system. Any validation errors will return a `422` and an object with a list of validation errors.

### Required Properties

{:.table.table-striped}
| Property | Type | Description | Example |
|----------|------|-------------|---------|
| name | string | The name of the choice list. | `"Inspection Conditions"`
| choices | array of choice objects | The choice list options. | See choices properties table below.

### Choices Properties

{:.table.table-striped}
| Property | Type | Required | Readonly | Description |
|----------|------|----------|----------|-------------|
| label | string | yes | no | Choice label. |
| value | string | no | no | Choice value, stored in the database. |

Example validation response if `choices` is not included:

```
{
  "choice_list": {
    "errors": {
      "choices": ["must be an array object"]
    }
  }
}
```

## Notes

* The entire choice_list object is required when making an update. Omitting fields with existing data will result in data loss! The typical workflow for updating an existing choice list is to fetch the choice_list object, modify it, and then submit the PUT request.

## Examples

### Valid choice list response

```json
{
  "choice_list": {
    "name": "Bridge Inspection Conditions",
    "description": "",
    "version": 1,
    "id": "56734070-0042-4f95-a914-b0307e65f9f3",
    "created_at": "2014-08-28T14:03:30Z",
    "updated_at": "2014-08-28T14:03:30Z",
    "choices": [{
      "label": "Excellent",
      "value": "Excellent"
    },
    {
      "label": "Good",
      "value": "Good"
    },
    {
      "label": "Fair",
      "value": "Fair"
    },
    {
      "label": "Poor",
      "value": "Poor"
    },
    {
      "label": "Unrated",
      "value": "Unrated"
    },
    {
      "label": "N/A",
      "value": "N/A"
    }]
  }
}
```

### Get all choice lists

#### cURL
```sh
curl --request GET 'https://api.fulcrumapp.com/api/v2/choice_lists.json' \
--header 'Accept: application/json' \
--header 'X-ApiToken: my-api-key'
```

#### jQuery
```js
$.ajax({
  type: "GET",
  url: "https://api.fulcrumapp.com/api/v2/choice_lists.json",
  contentType: "application/json",
  dataType: "json",
  headers: {
    "X-ApiToken": "my-api-key"
  },
  success: function (data) {
    // do something!
    console.log(data);
  }
});
```

### Get a single choice list by ID

#### cURL
```sh
curl --request GET 'https://api.fulcrumapp.com/api/v2/choice_lists/my-choice-list-id.json' \
--header 'Accept: application/json' \
--header 'X-ApiToken: my-api-key'
```

#### jQuery
```js
$.ajax({
  type: "GET",
  url: "https://api.fulcrumapp.com/api/v2/choice_lists/my-choice-list-id.json",
  contentType: "application/json",
  dataType: "json",
  headers: {
    "X-ApiToken": "my-api-key"
  },
  success: function (data) {
    // do something!
    console.log(data);
  }
});
```

### Create a new choice list

#### cURL
```sh
curl --request POST 'https://api.fulcrumapp.com/api/v2/choice_lists.json' \
--header 'Accept: application/json' \
--header 'Content-Type: application/json' \
--header 'X-ApiToken: my-api-key' \
--data '{"choice_list": {"name": "Bridge Inspection Conditions","choices": [{"label": "Excellent","value": "Excellent"},{"label": "Good","value": "Good"},{"label": "Fair","value": "Fair"},{"label": "Poor","value": "Poor"},{"label": "Unrated","value": "Unrated"},{"label": "N/A","value": "N/A"}]}}'
```

#### jQuery
```js
$.ajax({
  type: "POST",
  url: "https://api.fulcrumapp.com/api/v2/choice_lists.json",
  data: JSON.stringify({
    "choice_list": {
      "name": "Bridge Inspection Conditions",
      "choices": [{
        "label": "Excellent",
        "value": "Excellent"
      },
      {
        "label": "Good",
        "value": "Good"
      },
      {
        "label": "Fair",
        "value": "Fair"
      },
      {
        "label": "Poor",
        "value": "Poor"
      },
      {
        "label": "Unrated",
        "value": "Unrated"
      },
      {
        "label": "N/A",
        "value": "N/A"
      }]
    }
  }),
  contentType: "application/json",
  dataType: "json",
  headers: {
    "X-ApiToken": "my-api-key"
  },
  success: function (data) {
    // do something!
    console.log(data);
  }
});
```

### Update a choice list

#### cURL
```sh
curl --request PUT 'https://api.fulcrumapp.com/api/v2/choice_lists/my-choice-list-id.json' \
--header 'Accept: application/json' \
--header 'Content-Type: application/json' \
--header 'X-ApiToken: my-api-key' \
--data '{"choice_list": {"name": "Bridge Inspection Conditions","choices": [{"label": "Excellent","value": "Excellent"},{"label": "Good","value": "Good"},{"label": "Fair","value": "Fair"},{"label": "Poor","value": "Poor"},{"label": "Needs Replacing","value": "replace"},{"label": "Unrated","value": "Unrated"},{"label": "N/A","value": "N/A"}]}}'
```

#### jQuery
```js
$.ajax({
  type: "PUT",
  url: "https://api.fulcrumapp.com/api/v2/choice_lists/my-choice-list-id.json",
  data: JSON.stringify({
    "choice_list": {
      "name": "Bridge Inspection Conditions",
      "choices": [{
        "label": "Excellent",
        "value": "excellent"
      },
      {
        "label": "Good",
        "value": "good"
      },
      {
        "label": "Fair",
        "value": "fair"
      },
      {
        "label": "Poor",
        "value": "poor"
      },
      {
        "label": "Needs Replacing",
        "value": "replace"
      },
      {
        "label": "Unrated",
        "value": "unrated"
      },
      {
        "label": "N/A",
        "value": "na"
      }]
    }
  }),
  contentType: "application/json",
  dataType: "json",
  headers: {
    "X-ApiToken": "my-api-key"
  },
  success: function (data) {
    // do something!
    console.log(data);
  }
});
```

### Delete a choice list

#### cURL
```sh
curl --request DELETE 'https://api.fulcrumapp.com/api/v2/choice_lists/my-choice-list-id.json' \
--header 'Accept: application/json' \
--header 'X-ApiToken: my-api-key'
```

#### jQuery
```js
$.ajax({
  type: "DELETE",
  url: "https://api.fulcrumapp.com/api/v2/choice_lists/my-record-id.json",
  contentType: "application/json",
  dataType: "json",
  headers: {
    "X-ApiToken": "my-api-key"
  },
  success: function (data) {
    // do something!
    console.log(data);
  }
});
```
