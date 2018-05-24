---
layout: default
section: endpoints
order: 9
title: "Signatures"
description: "Fetch and upload signatures"
category: section
img: /assets/img/guides/
difficulty: advanced
menu:
  - "Endpoints": endpoints
  - "Query Parameters": query-parameters
  - "Properties": signature-properties
  - "Validations": validations
  - "Notes": notes
  - "Examples": examples
---

The Signatures API gives you access to a record's signatures. In order to upload a record with signatures, you must upload each signature individually. By default, fetching the records will generate the URLs for the signature, if a signature exists for that record.

## Endpoints

{:.table.table-striped}
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/v2/signatures/.json | Fetch the metadata for all signatures. |
| GET | /api/v2/signatures/**:id**.json | Fetch the metadata for a single signature. |
| GET | /api/v2/signatures/**:id**.png | Fetch the image for a single signature. |
| GET | /api/v2/signatures/**:id**/thumbnail.json | Fetch the thumbnail version metadata for a single signature. |
| GET | /api/v2/signatures/**:id**/thumbnail.png | Fetch the thumbnail version image for a single signature. |
| POST | /api/v2/signatures.json | Upload a single signature. |

## Query Parameters

Available parameters to query the signatures in your account. All of the parameters may be used together to filter your signatures for more accurate results.

{:.table.table-striped}
| Parameter | Type | Description |
|-----------|------|-------------|
| form_id | string | The id of the form with which the signature is associated. Leaving this blank will query against all of your signatures. |
| newest_first | boolean | If present, signatures will be sorted by `updated_at` date.
| page | integer | The page number requested. |
| per_page | integer | Number of records per page. By default, all requests are paginated to the maximum value of 20,000 items per request. |
| processed | boolean | Signature has been completely processed. |
| record_id | string | The id of the record with which the signature is associated. |
| stored | boolean | Signature has been completely stored. |
| uploaded | boolean | Signature has been completely uploaded. |

## Signature Properties

{:.table.table-striped}
| Property | Type | Required | Readonly | Description |
|----------|------|----------|----------|-------------|
| access_key | string | yes | no | The id of the signature. |
| created_at | string | no | yes | Timestamp when the signature was created. |
| updated_at | string | no | yes | Timestamp when the signature was last updated. |
| created_by | string | no | yes | The name of user who created the signature. |
| created_by_id | string | no | yes | The id of user who created the signature. |
| updated_by | string | no | yes | The name of user who last updated the signature. |
| updated_by_id | string | no | yes | The id of user who last updated the signature. |
| uploaded | boolean | no | yes | The file has been uploaded, but might not be fully stored on the backend yet. This is the least useful indicator unless you're writing a synchronizer |
| stored | boolean | no | yes | The `original` attribute is available for download. |
| processed | boolean | no | yes | The additional versions of the media are available for download. (thumbnails or small versions). |
| record_id | string | no | yes | The id of the record the signature is associated with. |
| form_id | string | no | yes | The id of the form the signature is associated with. |
| file_size | number | no | yes | The size of the signature file in bytes. |
| content_type | string | no | yes | The content type of the signature file. |
| url | string | no | yes | The URL to access the signature. |
| thumbnail | string | no | yes | The URL to access the thumbnail version of the signature. |
| large | string | no | yes | The URL to access the large version of the signature. |
| original | string | no | yes | The URL to access the original version of the signature. |

## Validations

The following properties must be included in order to create/update a photo object in our system. Any validation errors will return a `422` and an object with a list of validation errors.

### Required Properties

{:.table.table-striped}
| Property | Type | Description | Example |
|----------|------|-------------|---------|
| signature[access_key] | string | The id of the signature. | `"9855e3f2-85a5-4b9f-9e62-0b1bbcfef091"` |
| signature[file] | multipart/form-data | The signature file. | See [example](#upload-a-new-signature) below. |

Example validation response if `access_key` is not included:

```json
{
  "signatures": {
    "errors": {
      "access_key": ["must be provided"]
    }
  }
}
```

## Notes

* There is no `DELETE` method for signatures. Signatures can be effectively deleted by unlinking them from their associated record.

## Examples

### Valid signature

```json
{
  "signature": {
    "access_key": "9855e3f2-85a5-4b9f-9e62-0b1bbcfef091",
    "created_at": "2015-07-09T14:54:32Z",
    "updated_at": "2015-07-09T14:54:32Z",
    "uploaded": true,
    "stored": true,
    "processed": true,
    "record_id": "215f4147-51b5-42bb-84af-06b8112d9ecf",
    "form_id": "f99c0c14-7861-40b5-882c-d33fe2bcdeb2",
    "file_size": 22826,
    "content_type": "image/png",
    "url": "https://api.fulcrumapp.com/api/v2/signatures/9855e3f2-85a5-4b9f-9e62-0b1bbcfef091",
    "created_by": "Bryan McBride",
    "created_by_id": "50633f84a934480d260001db",
    "updated_by": "Bryan McBride",
    "updated_by_id": "50633f84a934480d260001db",
    "thumbnail": "https://fulcrumapp.s3.amazonaws.com/signatures/thumb_e063496f-3288-4d2b-ab54-2121e23de722-9855e3f2-85a5-4b9f-9e62-0b1bbcfef091.png",
    "large": "https://fulcrumapp.s3.amazonaws.com/signatures/large_e063496f-3288-4d2b-ab54-2121e23de722-9855e3f2-85a5-4b9f-9e62-0b1bbcfef091.png",
    "original": "https://fulcrumapp.s3.amazonaws.com/signatures/e063496f-3288-4d2b-ab54-2121e23de722-9855e3f2-85a5-4b9f-9e62-0b1bbcfef091.png"
  }
}
```

### Get metadata for all signatures

#### cURL
```sh
curl --request GET 'https://api.fulcrumapp.com/api/v2/signatures.json' \
--header 'Accept: application/json' \
--header 'X-ApiToken: my-api-key'
```

#### jQuery
```js
$.ajax({
  type: "GET",
  url: "https://api.fulcrumapp.com/api/v2/signatures.json",
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

### Get metadata for a single signature

#### cURL
```sh
curl --request GET 'https://api.fulcrumapp.com/api/v2/signatures/my-signature-access-key.json' \
--header 'Accept: application/json' \
--header 'X-ApiToken: my-api-key'
```

#### jQuery
```js
$.ajax({
  type: "GET",
  url: "https://api.fulcrumapp.com/api/v2/signatures/my-signature-access-key.json",
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


### Upload a new signature

```html
<form>
  <input type="file" name="signature" id="signature" /><br /><br />
  <input type="button" id="upload" value="upload" />
</form>
```

```js
$(document).ready(function(){
  /**
  * Generates a GUID string.
  * @returns {String} The generated GUID.
  * @example af8a8416-6e18-a307-bd9c-f2c947bbb3aa
  * @author Slavik Meltser (slavik@meltser.info).
  * @link http://slavik.meltser.info/?p=142
  **/
  function guid() {
    function _p8(s) {
      var p = (Math.random().toString(16)+"000000000").substr(2,8);
      return s ? "-" + p.substr(0,4) + "-" + p.substr(4,4) : p ;
    }
    return _p8() + _p8(true) + _p8(true) + _p8();
  }

  $("#upload").click(function(){
    var formData = new FormData();
    formData.append("signature[access_key]", guid());
    formData.append("signature[file]", $("#signature")[0].files[0]);
    $.ajax({
      type: "POST",
      url: "https://api.fulcrumapp.com/api/v2/signatures.json",
      data: formData,
      cache: false,
      contentType: false,
      processData: false,
      dataType: "json",
      headers: {
        "X-ApiToken": "my-api-key"
      },
      success: function (data) {
        // do something!
        console.log(data);
      }
    });
  });
});
```
