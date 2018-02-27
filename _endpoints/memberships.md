---
layout: default
section: endpoints
order: 15
title: "Memberships"
description: "Read, update, or delete project, layers, and form memberships"
category: section
img: /assets/img/guides/
difficulty: advanced
menu:
  - "Endpoints": endpoints
  - "Notes": notes
  - "Examples": examples
---

The memberships API provides access to your projects memberships. All methods require authentication via an API key as either an HTTP request header or query string parameter. The API allows you to see lists of users associated with each project, form, or layer, with a particular project, form, or layer, and to add and delete user membership in particularproject, form, or layer.

## Endpoints

{:.table.table-striped}
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/v2/memberships.json | Fetch all members in your organization |
| GET | /api/v2/memberships.json?form_id=**:id** | Fetch all members for a given form |
| GET | /api/v2/memberships.json?project_id=**:id** | Fetch all members for a given project |
| GET | /api/v2/memberships.json?layer_id=**:id** | Fetch memberships for a particular layer |
| POST | /api/v2/memberships/change_permissions.json | Add or remove membership permissions from layers, forms, or projects |

## Notes
 - The 'change permissions' endpoint accepts a json object labeled 'change' with three parameters
 - The first parameter is type, which indicates which object you want to change the membership permisisons of. Valid types are 'project_members', 'form_members', and 'layer_members'
 - The second parameter is the id of the object you want to change the permission of. Valid ID keys are 'project_id', 'form_id' and 'layers_id'
 - The third parameter is the change-set you would like to add or remove. This is a key of either 'add' or 'remove' with the value an array of ids that you would like to alter.

## Examples

form_id: 292a22c5-456a-4deb-a7d6-42ac409897b5
membership_id from michael@sni.io: b3170114-8a7d-490a-95a0-15d42492b429
membership_id from robbstark@harrenhall.com: 49ee5c1a-220f-4deb-8a9b-775706f66433


Below is an example of using curl to fetch a list of memberships from a given project.
```bash
curl -H "X-ApiToken: token" -H "Content-Type: application/json" https://api.fulcrumapp.com/api/v2/memberships.json?project_id=3751f03b-043d-48bf-a6cc-747ddd36b777
```

Below is an example of a curl to post to add a new membership to a given form.
```bash
curl -XPOST -H "X-ApiToken: token" -H "Content-Type: application/json" -d@- https://api.fulcrumapp.com/api/v2/memberships/change_permissions.json <<EOF
{
  "change": {
    "type": "form_members",
    "form_id": "292a22c5-456a-4deb-a7d6-42ac409897b5",
    "add": [ "49ee5c1a-220f-4deb-8a9b-775706f66433" ]
  }
}
EOF
```

Below is an example of a curl to post to add a new membership to a given form.
```bash
curl -XPOST -H "X-ApiToken: token" -H "Content-Type: application/json" -d@- https://api.fulcrumapp.com/api/v2/memberships/change_permissions.json <<EOF
{
  "change": {
    "type": "form_members",
    "form_id": "292a22c5-456a-4deb-a7d6-42ac409897b5",
    "remove": [ "49ee5c1a-220f-4deb-8a9b-775706f66433" ]
  }
}
EOF
```

## Valid Membership Response
```json
{
    "memberships": [
         {
            "id": "49ee5c1a-220f-4deb-8a9b-775706f66433",
            "created_at": "2018-01-19T23:38:39Z",
            "updated_at": "2018-01-19T23:38:39Z",
            "gravatar_email": null,
            "gravatar_image_url": "https://s.gravatar.com/avatar/d41d8cd98f00b204e9800998ecf8427e?s=80",
            "user_id": "8e20fc7e-33c3-4a34-9b13-167b24ebe89c",
            "user": "Robb Stark",
            "first_name": "Robb",
            "last_name": "Stark",
            "email": "robbstark@harrenhall.com",
            "role_id": "5a326552-865c-4c91-a45b-4262d4b6aedf",
            "image_small": "https://s.gravatar.com/avatar/d41d8cd98f00b204e9800998ecf8427e?s=300",
            "image_large": "https://s.gravatar.com/avatar/d41d8cd98f00b204e9800998ecf8427e?s=600"
        },
        {
            "id": "b21cc100-08b1-41ba-8353-d6e7a97debee",
            "created_at": "2018-01-19T23:38:39Z",
            "updated_at": "2018-01-22T18:48:46Z",
            "gravatar_email": null,
            "gravatar_image_url": "https://s.gravatar.com/avatar/d41d8cd98f00b204e9800998ecf8427e?s=80",
            "user_id": "c535156f-2871-4084-a399-d3061b25fae4",
            "user": "Walder Frey",
            "first_name": "Walder",
            "last_name": "Frey",
            "email": "walderfrey@thetwins.com",
            "role_id": "5696b7e9-765f-42b4-a61c-2895f257aab3",
            "image_small": "https://s.gravatar.com/avatar/d41d8cd98f00b204e9800998ecf8427e?s=300",
            "image_large": "https://s.gravatar.com/avatar/d41d8cd98f00b204e9800998ecf8427e?s=600"
        }
    ]
}
```