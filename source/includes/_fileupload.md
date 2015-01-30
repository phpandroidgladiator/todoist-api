# File uploads

> On success, an HTTP 200 OK with JSON data of file data is returned:

```shell
$ curl https://todoist.com/API/v6/upload_file -X POST \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d file_name=example.jpg \
    --data-binary @example.jpg
{
  "file_name": "example.jpg",
  "file_size": 85665,
  "file_type": "image/jpeg",
  "file_url": "https://*.cloudfront.net/*/example.jpg",
  "tn_l": [
    "https://*.cloudfront.net/tn_l_*.jpg",
    400, 309
  ],
  "tn_m": [
    "https://*.cloudfront.net/tn_m_*.jpg",
    288, 222
  ],
  "tn_s": [
    "https://*.cloudfront.net/tn_s_*.jpg",
    96, 74
  ]
}
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.upload_file('example.jpg')
{
  'file_name': 'example.jpg',
  'file_size': 85665,
  'file_type': 'image/jpeg',
  'file_url': 'https://*.cloudfront.net/*/example.jpg',
  'tn_l': [
    'https://*.cloudfront.net/tn_l_*.jpg',
    400, 309
  ],
  'tn_m': [
    'https://*.cloudfront.net/tn_m_*.jpg',
    288, 222
  ],
  'tn_s': [
    'https://*.cloudfront.net/tn_s_*.jpg',
    96, 74
  ]
}

```
Upload a file suitable to be passed as a `file_attachment` attribute to the `note_add` or `note_update` calls.

### Required parameters

Parameter | Description
--------- | -----------
token | The user's token (received on login).
file_name | The file name to be uploaded.

### Base file properties

Attribute | Description
--------- | -----------
file_name | The name of the file
file_size | The size of the file in bytes
file_type | MIME type
file_url | The URL where the file is located. Note that we don't cache the remote content on our servers and stream or expose files directly from third party resources. In particular this means that you should avoid providing links to non-encrypted (plain HTTP) respources, as exposing this files in Todoist may issue a browser warning.
upload_state | Upload completion state

### Image file properties

If you upload an image, you may provide thumbnail paths to ensure Todoist handles them appropriately. Valid thumbnail information is a JSON array with URL, width in pixels, height in pixels. Ex.: `["http://example.com/img.jpg",400,300]`. "Canonical" thumbnails (ones we create by `uploadFile` API call) have following sizes: `96x96`, `288x288`, `528x528`.

Attribute | Description
--------- | -----------
tn_l | Large thumbnail.
tn_m | Medium thumbnail.
tn_s | Small thumbnail.

### Audio file properties

If you upload an audio file, you may provide an extra attribute `file_duration` (duration of the audio file in seconds, which takes an integer value). In the web interface the file is rendered back with a `<audio>` tag, so you should make sure it's supported in current web browsers. See [supported media formats](https://developer.mozilla.org/en-US/docs/HTML/Supported_media_formats) for the reference.
