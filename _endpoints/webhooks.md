---
layout: default
section: endpoints
order: 13
title: "Webhooks"
description: "Create, read, update, or delete webhooks"
category: section
img: /assets/img/guides/
difficulty: advanced
menu:
  - "Endpoints": endpoints
  - "Properties": webhook-properties
  - "Validations": validations
  - "Notes": notes
  - "Examples": examples
---

The Webhooks API gives you access to manage your organization's webhooks programmatically.

For more general information about webhooks, or details of responsibilities your webhook endpoint must meet, see the [Webhooks Overview](/general/webhooks/).

## Endpoints

{:.table.table-striped}
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/v2/webhooks.json | Fetch all webhooks. |
| GET | /api/v2/webhooks/**:id**.json | Fetch a single webhook. |
| POST | /api/v2/webhooks.json | Create a single webhook. |
| PUT | /api/v2/webhooks/**:id**.json | Update a single webhook. |
| DELETE | /api/v2/webhooks/**:id**.json | Delete a single webhook. |

## Webhook Properties

{:.table.table-striped}
| Property | Type | Required | Readonly | Description |
|----------|------|----------|----------|-------------|
| name | string | yes | no | The name of the webhook. |
| url | string | yes | no | The HTTP URL that will receive event requets as they occur. |
| active | boolean | no | no | Default: true. If true, this webhook will receive event requests. If false, this webhook will not receive event requests. |
| id | string | no | yes | The id of the webhook. |
| created_at | string | no | yes | Timestamp when the webhook was created. |
| updated_at | string | no | yes | Timestamp when the webhook was last updated. |

## Validations

The following properties must be included in order to create/update a webhook object in our system. Any validation errors will return a `422` and an object with a list of validation errors.

### Required Properties

{:.table.table-striped}
| Property | Type | Description | Example |
|----------|------|-------------|---------|
| name | string | The name of the choice webhook. | `"Fire Hydrant Inventory Emails"`
| url | string | The HTTP URL that will receive event requets as they occur. | `"https://my-webhook-processing-script.php"`

Example validation response if `url` is not included:

```json
{
  "webhook": {
    "errors": {
      "url": ["can't be blank"]
    }
  }
}
```

## Notes

* Your [plan must enable webhooks](http://help.fulcrumapp.com/setting-up-webhooks/how-to-enable-webhooks) and you must also have an account with the [right role/permission](http://help.fulcrumapp.com/setting-up-webhooks/roles-and-permissions) to manage webhooks.

## Examples

### Valid webhook

```json
{
  "webhook": {
    "name": "Fire Hydrant Inventory Emails",
    "url": "https://script.google.com/macros/s/AKfycbyrR_bJr0XEW30DbDEXT3yjle5akRa9Dci7q6H0VTA_Oov86Vj/exec",
    "active": true,
    "id": "1617ae52-0f8a-463f-bfc0-89787c6474ea",
    "created_at": "2014-08-19T20:24:31Z",
    "updated_at": "2014-12-17T04:28:05Z"
  }
}
```

### Get all webhooks

#### cURL
```sh
curl --request GET 'https://api.fulcrumapp.com/api/v2/webhooks.json' \
--header 'Accept: application/json' \
--header 'X-ApiToken: my-api-key'
```

#### jQuery
```js
$.ajax({
  type: "GET",
  url: "https://api.fulcrumapp.com/api/v2/webhooks.json",
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

### Get a single webhook

#### cURL
```sh
curl --request GET 'https://api.fulcrumapp.com/api/v2/webhooks/my-webhook-id.json' \
--header 'Accept: application/json' \
--header 'X-ApiToken: my-api-key'
```

#### jQuery
```js
$.ajax({
  type: "GET",
  url: "https://api.fulcrumapp.com/api/v2/webhooks/my-webhook-id.json",
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

### Create a new webhook

#### cURL
```sh
curl --request POST 'https://api.fulcrumapp.com/api/v2/webhooks.json' \
--header 'Accept: application/json' \
--header 'Content-Type: application/json' \
--header 'X-ApiToken: my-api-key' \
--data '{"webhook": {"name": "Fire Hydrant Inventory Emails","url": "https://my-webhook-processing-script.php"}}'
```

#### jQuery
```js
$.ajax({
  type: "POST",
  url: "https://api.fulcrumapp.com/api/v2/webhooks.json",
  data: JSON.stringify({
    "webhook": {
      "name": "Fire Hydrant Inventory Emails",
      "url": "https://my-webhook-processing-script.php"
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

### Update a webhook

#### cURL
```sh
curl --request PUT 'https://api.fulcrumapp.com/api/v2/webhooks/my-webhook-id.json' \
--header 'Accept: application/json' \
--header 'Content-Type: application/json' \
--header 'X-ApiToken: my-api-key' \
--data '{"webhook": {"name": "Fire Hydrant Inventory Emails","url": "https://my-webhook-processing-script.php","active": false}}'
```

#### jQuery
```js
$.ajax({
  type: "PUT",
  url: "https://api.fulcrumapp.com/api/v2/webhooks/my-webhook-id.json",
  data: JSON.stringify({
    "webhook": {
      "name": "Fire Hydrant Inventory Emails",
      "url": "https://my-webhook-processing-script.php",
      "active": false
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

### Delete a webhook

#### cURL
```sh
curl --request DELETE 'https://api.fulcrumapp.com/api/v2/webhooks/my-webhook-id.json' \
--header 'Accept: application/json' \
--header 'X-ApiToken: my-api-key'
```

#### jQuery
```js
$.ajax({
  type: "DELETE",
  url: "https://api.fulcrumapp.com/api/v2/webhooks/my-webhook-id.json",
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
