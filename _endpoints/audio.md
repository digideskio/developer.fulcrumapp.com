---
layout: default
section: endpoints
order: 11
title: "Audio"
description: "Fetch and upload audio files and their GPS tracks"
category: section
img: /assets/img/guides/
difficulty: advanced
menu:
  - "Endpoints": endpoints
  - "Query Parameters": query-parameters
  - "Properties": audio-properties
  - "Validations": validations
  - "Notes": notes
  - "Examples": examples
---

The Audio API gives you access to a record's audio files, including the GPS track. In order to upload a record with audio attachments, you must first upload each audio file individually. By default, fetching the records will generate the URLs for the audio, if a recording exists for that record.

## Endpoints

{:.table.table-striped}
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/v2/audio.json | Fetch the metadata for all audio. |
| GET | /api/v2/audio/tracks.json | Fetch the GPS tracks for all audio as simple JSON. |
| GET | /api/v2/audio/tracks.geojson | Fetch the GPS tracks for all audio as GeoJSON. |
| GET | /api/v2/audio/tracks.geojson?type=points | Fetch the GPS tracks for all audio as GeoJSON points. |
| GET | /api/v2/audio/tracks.gpx | Fetch the GPS tracks for all audio as GPX. |
| GET | /api/v2/audio/tracks.kml | Fetch the GPS tracks for all audio as KML. |
| GET | /api/v2/audio/**:id**.json | Fetch the metadata for a single audio. |
| GET | /api/v2/audio/**:id**.mp4 | Fetch the audio for a single audio. |
| GET | /api/v2/audio/**:id**/track.json | Fetch the GPS track for a single audio as simple JSON. |
| GET | /api/v2/audio/**:id**/track.geojson | Fetch the GPS track for a single audio as GeoJSON. |
| GET | /api/v2/audio/**:id**/track.geojson?type=points | Fetch the GPS track for a single audio as GeoJSON points. |
| GET | /api/v2/audio/**:id**/track.gpx | Fetch the GPS track for a single audio as GPX. |
| GET | /api/v2/audio/**:id**/track.kml | Fetch the GPS track for a single audio as KML. |
| POST | /api/v2/audio/upload | Upload a single audio recording (.m4a) or a single audio GPS track (.json). |

## Query Parameters

Available parameters to query the audio files in your account. All of the parameters may be used together to filter your audio files for more accurate results.

{:.table.table-striped}
| Parameter | Type | Description |
|-----------|------|-------------|
| form_id | string | The id of the form with which the audio file is associated. Leaving this blank will query against all of your audio file. |
| newest_first | boolean | If present, audio files will be sorted by `updated_at` date.
| page | integer | The page number requested. |
| per_page | integer | Number of records per page. By default, all requests are paginated to the maximum value of 20,000 items per request. |
| processed | boolean | Audio file has been completely processed. |
| record_id | string | The id of the record with which the audio file is associated. |
| stored | boolean | Audio file has been completely stored. |
| uploaded | boolean | Audio file has been completely uploaded. |

## Audio Properties

{:.table.table-striped}
| Property | Type | Required | Readonly | Description |
|----------|------|----------|----------|-------------|
| access_key | string | yes | no | The id of the audio. |
| created_at | string | no | yes | Timestamp when the audio was created. |
| updated_at | string | no | yes | Timestamp when the audio was last updated. |
| created_by | string | no | yes | The name of user who created the audio. |
| created_by_id | string | no | yes | The id of user who created the audio. |
| updated_by | string | no | yes | The name of user who last updated the audio. |
| updated_by_id | string | no | yes | The id of user who last updated the audio. |
| uploaded | boolean | no | yes | The file has been uploaded, but might not be fully stored on the backend yet. This is the least useful indicator unless you're writing a synchronizer. |
| stored | boolean | no | yes | The `original` attribute is available for download. |
| processed | boolean | no | yes | The additional versions of the media are available for download. (thumbnails or small versions). |
| record_id | string | no | yes | The id of the record the audio is associated with. |
| form_id | string | no | yes | The id of the form the audio is associated with. |
| file_size | number | no | yes | The size of the audio file in bytes. |
| content_type | string | no | yes | The content type of the audio file. |
| url | string | no | yes | The URL to access the audio recording. |
| track | string | no | yes | The URL to access the audio GPS track. |
| metadata | metadata object | no | yes | The audio's metadata (varies by device). |

## Validations

The following properties must be included in order to create/update an audio object in our system. Any validation errors will return a `422` and an object with a list of validation errors.

### Required Properties

{:.table.table-striped}
| Property | Type | Description | Example |
|----------|------|-------------|---------|
| audio[access_key] | string | The id of the audio. | `"2d956eb0-bc2a-747f-fc56-1100936ce515"` |
| audio[file] | multipart/form-data | The audio file. | See [example](#upload-a-new-audio) below. |

Example validation response if `access_key` is not included:

```json
{
  "audio": {
    "errors": {
      "access_key": ["must be provided"]
    }
  }
}
```

## Notes

* There is no `DELETE` method for audio. Audio files can be effectively deleted by unlinking them from their associated record.

## Examples

### Valid audio

```json
{
  "audio": {
    "access_key": "f0eb217d-3d4b-4ade-81b7-bac63788f396",
    "created_at": "2016-05-13T13:48:42Z",
    "updated_at": "2016-05-13T13:48:44Z",
    "uploaded": true,
    "stored": true,
    "processed": true,
    "record_id": "1bfe797c-ad97-4f25-bf5c-d83a02d0145c",
    "form_id": "33d2f8d7-9aea-41a6-825b-f55da4b68a4b",
    "file_size": 558962,
    "content_type": "audio/m4a",
    "url": "https://api.fulcrumapp.com/api/v2/audio/f0eb217d-3d4b-4ade-81b7-bac63788f396",
    "track": "https://api.fulcrumapp.com/api/v2/audio/f0eb217d-3d4b-4ade-81b7-bac63788f396/track.json",
    "created_by": "Bryan McBride",
    "created_by_id": "50633f84a934480d260001db",
    "updated_by": "Bryan McBride",
    "updated_by_id": "50633f84a934480d260001db",
    "metadata": {
      "streams": [{
        "index": 0,
        "codec_name": "aac",
        "codec_long_name": "AAC (Advanced Audio Coding)",
        "profile": "LC",
        "codec_type": "audio",
        "codec_time_base": "1/44100",
        "codec_tag_string": "mp4a",
        "codec_tag": "0x6134706d",
        "sample_fmt": "fltp",
        "sample_rate": "44100",
        "channels": 1,
        "channel_layout": "mono",
        "bits_per_sample": 0,
        "r_frame_rate": "0/0",
        "avg_frame_rate": "0/0",
        "time_base": "1/44100",
        "start_pts": 0,
        "start_time": "0.000000",
        "duration_ts": 2924479,
        "duration": "66.314717",
        "bit_rate": "65556",
        "max_bit_rate": "96000",
        "nb_frames": "2856",
        "disposition": {
          "default": 1,
          "dub": 0,
          "original": 0,
          "comment": 0,
          "lyrics": 0,
          "karaoke": 0,
          "forced": 0,
          "hearing_impaired": 0,
          "visual_impaired": 0,
          "clean_effects": 0,
          "attached_pic": 0
        },
        "tags": {
          "creation_time": "2016-05-12 21:32:59",
          "language": "eng",
          "handler_name": "SoundHandle"
        }
      }],
      "format": {
        "nb_streams": 1,
        "nb_programs": 0,
        "format_name": "mov,mp4,m4a,3gp,3g2,mj2",
        "format_long_name": "QuickTime / MOV",
        "start_time": "0.000000",
        "duration": "66.315000",
        "size": "558962",
        "bit_rate": "67431",
        "probe_score": 100,
        "tags": {
          "major_brand": "mp42",
          "minor_version": "0",
          "compatible_brands": "isommp42",
          "creation_time": "2016-05-12 21:32:59"
        }
      }
    },
    "small": "https://fulcrumapp.s3.amazonaws.com/audio/small_9e10a61a-aec5-465e-9683-93e062b222ae-f0eb217d-3d4b-4ade-81b7-bac63788f396.m4a?AWSAccessKeyId=AKIAINLD3OFR6WK7JO3Q&Signature=IZ942FBS9NVE3J0ZkeapZX0Gdmg%3D&Expires=1510302466",
    "medium": "https://fulcrumapp.s3.amazonaws.com/audio/medium_9e10a61a-aec5-465e-9683-93e062b222ae-f0eb217d-3d4b-4ade-81b7-bac63788f396.m4a?AWSAccessKeyId=AKIAINLD3OFR6WK7JO3Q&Signature=6hZJVlq/wqgDvIgHdaXWzbXuOJ4%3D&Expires=1510302466",
    "original": "https://fulcrumapp.s3.amazonaws.com/audio/9e10a61a-aec5-465e-9683-93e062b222ae-f0eb217d-3d4b-4ade-81b7-bac63788f396.m4a?AWSAccessKeyId=AKIAINLD3OFR6WK7JO3Q&Signature=rq0G2MYhaOyH9a6lxy4UleYYsE0%3D&Expires=1510302466"
  }
}
```

### Valid audio track

```json
{
  "tracks": [{
    "track": [
      [1463088713169, null, null, null, null, null, null, null, 0.0, 0.0],
      [1463088718361, 42.8319061, -73.8938649, null, 18.569, null, null, null, 149.0, 0.0],
      [1463088723234, 42.8318624, -73.8938189, null, 19.889, null, null, null, 234.0, -21.0],
      [1463088727213, 42.8319061, -73.8938649, null, 19.112, null, null, null, 234.0, -21.0],
      [1463088733266, 42.8318624, -73.8938189, null, 19.906, null, null, null, 234.0, -23.0],
      [1463088738273, 42.8319061, -73.8938649, null, 19.015, null, null, null, 261.0, 37.0],
      [1463088742254, 42.8319061, -73.8938649, null, 18.802, null, null, null, 248.0, 28.0],
      [1463088748361, 42.8319061, -73.8938649, null, 19.108, null, null, null, 260.0, 38.0],
      [1463088753319, 42.8319061, -73.8938649, null, 19.003, null, null, null, 265.0, 43.0],
      [1463088757315, 42.8319061, -73.8938649, null, 18.827, null, null, null, 189.0, 0.0],
      [1463088763388, 42.8319061, -73.8938649, null, 18.904, null, null, null, 189.0, 0.0],
      [1463088769018, 42.8319061, -73.8938649, null, 19.045, null, null, null, 190.0, 0.0],
      [1463088773030, 42.8319061, -73.8938649, null, 50.0, null, null, null, 279.0, 57.0],
      [1463088779118, 42.8319061, -73.8938649, null, 50.0, null, null, null, 136.0, 0.0]
    ]
  }]
}
```

Each audio track consists of an array of track point arrays. Each track point array may include the following items in this order:
```
[timestamp (milliseconds since Unix Epoch), latitude, longitude, altitude, horizontal_accuracy, vertical_accuracy, course, speed, heading, inclination]
```
Each item should be a number or null and the only required items are timestamp, latitude, and longitude.

### Get the metadata for all audio files

#### cURL
```sh
curl --request GET 'https://api.fulcrumapp.com/api/v2/audio.json' \
--header 'Accept: application/json' \
--header 'X-ApiToken: my-api-key'
```

#### jQuery
```js
$.ajax({
  type: "GET",
  url: "https://api.fulcrumapp.com/api/v2/audio.json",
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

### Get the tracks for all audio files as GeoJSON

#### cURL
```sh
curl --request GET 'https://api.fulcrumapp.com/api/v2/audio/tracks.geojson' \
--header 'Accept: application/json' \
--header 'X-ApiToken: my-api-key'
```

#### jQuery
```js
$.ajax({
  type: "GET",
  url: "https://api.fulcrumapp.com/api/v2/audio/tracks.geojson",
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

### Get the metadata for a single audio file

#### cURL
```sh
curl --request GET 'https://api.fulcrumapp.com/api/v2/audio/my-audio-access-key.json' \
--header 'Accept: application/json' \
--header 'X-ApiToken: my-api-key'
```

#### jQuery
```js
$.ajax({
  type: "GET",
  url: "https://api.fulcrumapp.com/api/v2/audio/my-audio-access-key.json",
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

### Get the track for a single audio file as GeoJSON

#### cURL
```sh
curl --request GET 'https://api.fulcrumapp.com/api/v2/audio/my-audio-access-key/track.geojson' \
--header 'Accept: application/json' \
--header 'X-ApiToken: my-api-key'
```

#### jQuery
```js
$.ajax({
  type: "GET",
  url: "https://api.fulcrumapp.com/api/v2/audio/my-audio-access-key/track.geojson",
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

### Upload a new audio file

```html
<form>
  <input type="file" name="audio" id="audio" /><br /><br />
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
    formData.append("audio[access_key]", guid());
    formData.append("audio[file]", $("#audio")[0].files[0]);
    $.ajax({
      type: "POST",
      url: "https://api.fulcrumapp.com/api/v2/audio/upload",
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

### Upload a track for an existing audio file

```html
<form>
  <input type="file" name="track" id="track" /><br /><br />
  <input type="button" id="upload-existing" value="upload" />
</form>
```

```js
$(document).ready(function(){
  $("#upload-existing").click(function(){
    var formData = new FormData();
    formData.append("audio[access_key]", "my-audio-access-key");
    formData.append("audio[track]", $("#track")[0].files[0]);
    $.ajax({
      type: "POST",
      url: "https://api.fulcrumapp.com/api/v2/audio/upload",
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
