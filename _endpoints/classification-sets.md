---
layout: default
section: endpoints
order: 6
title: "Classification Sets"
description: "Create, read, update, or delete classification sets"
category: section
img: /assets/img/guides/
difficulty: advanced
menu:
  - "Endpoints": endpoints
  - "Query Parameters": query-parameters
  - "Properties": classification-set-properties
  - "Validations": validations
  - "Notes": notes
  - "Examples": examples
---

The Classification Sets API gives you access to the [classification sets](http://help.fulcrumapp.com/field-types/how-do-classification-sets-work) within your Fulcrum account. Classification sets are hierarchical lists of descriptors, allowing nested sets of options, which can be shared between multiple apps.

## Endpoints

{:.table.table-striped}
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET  | /api/v2/classification_sets.json | Fetch all classification sets. |
| GET | /api/v2/classification_sets/**:id**.json | Fetch a single classification set. |
| POST | /api/v2/classification_sets.json | Create a new classification set. |
| PUT | /api/v2/classification_sets/**:id**.json | Update a single classification set. |
| DELETE | /api/v2/classification_sets/**:id**.json | Delete a single classification set. |

## Query Parameters

{:.table.table-striped}
| Parameter | Type | Description |
|-----------|------|-------------|
| page | integer | The page number requested. |
| per_page | integer | Number of records per page. By default, all requests are paginated to the maximum value of 20,000 items per request. |

## Classification Set Properties

{:.table.table-striped}
| Property | Type | Required | Readonly | Description |
|----------|------|----------|----------|-------------|
| name | string | yes | no | The name of the classification set. |
| items | array of classification set objects | yes | no | The classification set options. |
| description | string | no | no | Optional classification set description. |
| version | number | no | yes | Autoincremented version of the choice list for history tracking. |
| id | string | no | yes | The id of the classification set. |
| created_at | string | no | yes | Timestamp when the choice list was created. |
| updated_at | string | no | yes | Timestamp when the choice list was last updated. |

## Validations

The following properties must be included in order to create/update a classification_set object in our system. Any validation errors will return a `422` and an object with a list of validation errors.

### Required Properties

{:.table.table-striped}
| Property | Type | Description | Example |
|----------|------|-------------|---------|
| name | string | The name of the choice list. | `"Inspection Conditions"`
| items | array of classification set objects | The classification set options. | See classification sets properties table below.

### Classification Sets Properties

{:.table.table-striped}
| Property | Type | Required | Readonly | Description |
|----------|------|----------|----------|-------------|
| label | string | yes | no | Choice label. |
| value | string | no | no | Choice value, stored in the database. |
| child_classifications | array of label/value/child_classifications objects | no | Child classifications associated with this parent. |

Example validation response if `items` array is not included:

```json
{
  "classification_set": {
    "errors": {
      "base": ["child_classifications must be an array object"]
    }
  }
}
```

## Notes

* The entire classificatio_set object is required when making an update. Omitting fields with existing data will result in data loss! The typical workflow for updating an existing classification set is to fetch the choice_list object, modify it, and then submit the PUT request.

## Examples

### Valid classification set

```json
{
  "classification_set": {
    "name": "Wildlife Types",
    "description": "",
    "version": 2,
    "id": "50379f51a9344807fe000022",
    "created_at": "2012-08-24T15:35:45Z",
    "updated_at": "2013-10-04T03:33:57Z",
    "items": [{
      "label": "Bird",
      "value": "Bird",
      "child_classifications": [{
        "label": "Cormorant",
        "value": "Cormorant"
      },
      {
        "label": "Egret",
        "value": "Egret"
      },
      {
        "label": "Frigate Bird",
        "value": "Frigate Bird"
      },
      {
        "label": "Heron",
        "value": "Heron"
      },
      {
        "label": "Osprey",
        "value": "Osprey"
      },
      {
        "label": "Pelican",
        "value": "Pelican"
      },
      {
        "label": "Pigeon",
        "value": "Pigeon"
      },
      {
        "label": "Tern",
        "value": "Tern"
      }]
    },
    {
      "label": "Butterfly",
      "value": "Butterfly"
    },
    {
      "label": "Fish",
      "value": "Fish"
    },
    {
      "label": "Manatee",
      "value": "Manatee"
    },
    {
      "label": "Shellfish",
      "value": "Shellfish",
      "child_classifications": [{
        "label": "Crab",
        "value": "Crab",
        "child_classifications": [{
          "label": "Blue Crab",
          "value": "Blue Crab"
        },
        {
          "label": "Fiddler Crab",
          "value": "Fiddler Crab"
        },
        {
          "label": "Hermit Crab",
          "value": "Hermit Crab"
        }]
      }]
    },
    {
      "label": "Turtle",
      "value": "Turtle"
    }]
  }
}
```

### Get all classification sets

#### cURL
```sh
curl --request GET 'https://api.fulcrumapp.com/api/v2/classification_sets.json' \
--header 'Accept: application/json' \
--header 'X-ApiToken: my-api-key'
```

#### jQuery
```js
$.ajax({
  type: "GET",
  url: "https://api.fulcrumapp.com/api/v2/classification_sets.json",
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

### Get a single classification set by ID

#### cURL
```sh
curl --request GET 'https://api.fulcrumapp.com/api/v2/classification_sets/my-classification-set-id.json' \
--header 'Accept: application/json' \
--header 'X-ApiToken: my-api-key'
```

#### jQuery
```js
$.ajax({
  type: "GET",
  url: "https://api.fulcrumapp.com/api/v2/classification_sets/my-classification-set-id.json",
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

### Create a new classification set

#### cURL
```sh
curl --request POST 'https://api.fulcrumapp.com/api/v2/classification_sets.json' \
--header 'Accept: application/json' \
--header 'Content-Type: application/json' \
--header 'X-ApiToken: my-api-key' \
--data '{"classification_set":{"name":"Wildlife Types","items":[{"label":"Bird","value":"Bird","child_classifications":[{"label":"Cormorant","value":"Cormorant"},{"label":"Egret","value":"Egret"},{"label":"Frigate Bird","value":"Frigate Bird"},{"label":"Heron","value":"Heron"},{"label":"Osprey","value":"Osprey"},{"label":"Pelican","value":"Pelican"},{"label":"Pigeon","value":"Pigeon"},{"label":"Tern","value":"Tern"}]},{"label":"Butterfly","value":"Butterfly"},{"label":"Fish","value":"Fish"},{"label":"Manatee","value":"Manatee"},{"label":"Shellfish","value":"Shellfish","child_classifications":[{"label":"Crab","value":"Crab","child_classifications":[{"label":"Blue Crab","value":"Blue Crab"},{"label":"Fiddler Crab","value":"Fiddler Crab"},{"label":"Hermit Crab","value":"Hermit Crab"}]}]},{"label":"Turtle","value":"Turtle"}]}}'
```

#### jQuery
```js
$.ajax({
  type: "POST",
  url: "https://api.fulcrumapp.com/api/v2/classification_sets.json",
  data: JSON.stringify({
    "classification_set": {
      "name": "Wildlife Types",
      "items": [{
        "label": "Bird",
        "value": "Bird",
        "child_classifications": [{
          "label": "Cormorant",
          "value": "Cormorant"
        },
        {
          "label": "Egret",
          "value": "Egret"
        },
        {
          "label": "Frigate Bird",
          "value": "Frigate Bird"
        },
        {
          "label": "Heron",
          "value": "Heron"
        },
        {
          "label": "Osprey",
          "value": "Osprey"
        },
        {
          "label": "Pelican",
          "value": "Pelican"
        },
        {
          "label": "Pigeon",
          "value": "Pigeon"
        },
        {
          "label": "Tern",
          "value": "Tern"
        }]
      },
      {
        "label": "Butterfly",
        "value": "Butterfly"
      },
      {
        "label": "Fish",
        "value": "Fish"
      },
      {
        "label": "Manatee",
        "value": "Manatee"
      },
      {
        "label": "Shellfish",
        "value": "Shellfish",
        "child_classifications": [{
          "label": "Crab",
          "value": "Crab",
          "child_classifications": [{
            "label": "Blue Crab",
            "value": "Blue Crab"
          },
          {
            "label": "Fiddler Crab",
            "value": "Fiddler Crab"
          },
          {
            "label": "Hermit Crab",
            "value": "Hermit Crab"
          }]
        }]
      },
      {
        "label": "Turtle",
        "value": "Turtle"
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

### Update a classification set

#### cURL
```sh
curl --request PUT 'https://api.fulcrumapp.com/api/v2/classification_sets/my-classification-set-id.json' \
--header 'Accept: application/json' \
--header 'Content-Type: application/json' \
--header 'X-ApiToken: my-api-key' \
--data '{"classification_set":{"name":"Wildlife Types","items":[{"label":"Bird","value":"Bird","child_classifications":[{"label":"Cormorant","value":"Cormorant"},{"label":"Egret","value":"Egret"},{"label":"Frigate Bird","value":"Frigate Bird"},{"label":"Heron","value":"Heron"},{"label":"Osprey","value":"Osprey"},{"label":"Pelican","value":"Pelican"},{"label":"Pigeon","value":"Pigeon"},{"label":"Tern","value":"Tern"}]},{"label":"Butterfly","value":"Butterfly"},{"label":"Fish","value":"Fish"},{"label":"Manatee","value":"Manatee"},{"label":"Shellfish","value":"Shellfish","child_classifications":[{"label":"Crab","value":"Crab","child_classifications":[{"label":"Blue Crab","value":"Blue Crab"},{"label":"Fiddler Crab","value":"Fiddler Crab"},{"label":"Hermit Crab","value":"Hermit Crab"}]}]},{"label":"Turtle","value":"Turtle"},{"label":"Frog","value":"Frog"}]}}'
```

#### jQuery
```js
$.ajax({
  type: "PUT",
  url: "https://api.fulcrumapp.com/api/v2/classification_sets/my-classification-set-id.json",
  data: JSON.stringify({
    "classification_set": {
      "name": "Wildlife Types",
      "items": [{
        "label": "Bird",
        "value": "Bird",
        "child_classifications": [{
          "label": "Cormorant",
          "value": "Cormorant"
        },
        {
          "label": "Egret",
          "value": "Egret"
        },
        {
          "label": "Frigate Bird",
          "value": "Frigate Bird"
        },
        {
          "label": "Heron",
          "value": "Heron"
        },
        {
          "label": "Osprey",
          "value": "Osprey"
        },
        {
          "label": "Pelican",
          "value": "Pelican"
        },
        {
          "label": "Pigeon",
          "value": "Pigeon"
        },
        {
          "label": "Tern",
          "value": "Tern"
        }]
      },
      {
        "label": "Butterfly",
        "value": "Butterfly"
      },
      {
        "label": "Fish",
        "value": "Fish"
      },
      {
        "label": "Manatee",
        "value": "Manatee"
      },
      {
        "label": "Shellfish",
        "value": "Shellfish",
        "child_classifications": [{
          "label": "Crab",
          "value": "Crab",
          "child_classifications": [{
            "label": "Blue Crab",
            "value": "Blue Crab"
          },
          {
            "label": "Fiddler Crab",
            "value": "Fiddler Crab"
          },
          {
            "label": "Hermit Crab",
            "value": "Hermit Crab"
          }]
        }]
      },
      {
        "label": "Turtle",
        "value": "Turtle"
      },
      {
        "label": "Frog",
        "value": "Frog"
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

### Delete a classification set

#### cURL
```sh
curl --request DELETE 'https://api.fulcrumapp.com/api/v2/classification_sets/my-classification-set-id.json' \
--header 'Accept: application/json' \
--header 'X-ApiToken: my-api-key'
```

#### jQuery
```js
$.ajax({
  type: "DELETE",
  url: "https://api.fulcrumapp.com/api/v2/classification_sets/my-classification-set-id.json",
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
