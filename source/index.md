---
title: emojidex API Reference

language_tabs:
  - shell
  - js
  - ruby

toc_footers:
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction
<aside class="notice">
This documenation is for Version 1 of the API.
</aside>

This documentation covers access and usage of emojidex service utilizing the API and CDN.

# API

## HTTP Requests
`https://www.emojidex.com/api/v1`

The API must always be called through HTTPS/SSL. This is to prevent leaking of user auth 
tokens and generally to keep as much information as to how users use emojidex as private 
as possible.

## HTTP Methods
Throughout most of the API different actions are mapped to different HTTP Methods, such as GET, 
PUT, POST, DELETE. You MUST be aware of what method you are using to make an API request, as most 
of the time only one HTTP method will perform the task you want in your query.

## API Keys
Coming soon. For now you do not need to set an API key.

## Authentication
Not all API calls require authentication, but all API calls can be performed with authentication 
tokens included. Generally if a user has an auth token it should be used with every call.

> User Authentication:

```ruby
```

```js
```

```shell
# With shell, you can just pass the correct header with each request
curl "www.emojidex.com/api/v1"
  -H ""
```

## Current emoji Index
Gets a general index of popular and new emoji.

```ruby
```

```js
```

```shell
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Isis",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```
#Assets

## Assets [CDN] Address
`http://assets.emojidex.com`

Note that this is not (currently) an HTTPS address. Assets are hosted on an S3 instance. 
In the future we will try to rig SSL up to the assets subdomain but at the moment SSL 
can optionally be used by allowing certs from the \*.s3.amazon.com address.

## Asset Access / Layout

Assets can be found on the Content Delivery Network under structured 
directories.

Each emoji asset is composed of the emoji code, with spaces represented as underscores. 
For example, the emoji code "blush" would have PNG images named "blush.png".

All emoji on www.emojidex.com can be found under /emoji, with the root /emoji containing SVG 
images:  
`http://assets.emojidex.com/emoji/blush.svg`  
![blush emoji](http://assets.emojidex.com/emoji/blush.svg)

PNG images come in a variety of sizes:  
```
{ ldpi: 9, mdpi: 18, hdpi: 27, xhdpi: 36, px8: 8,
  px16: 16, px32: 32, px64: 64, px128: 128, px256: 256 }
```  
Each emoji size is squared (wide emoji coming soon!). An emoji in a particular size is always
guarnteed to have the height specified for its size. For example, LDPI emoji are always 9px 
high.  

display | size | address
------- | ---- | -------
![blush emoji](http://assets.emojidex.com/emoji/ldpi/blush.png)  | ldpi  | http://assets.emojidex.com/emoji/ldpi/blush.png
![blush emoji](http://assets.emojidex.com/emoji/mdpi/blush.png)  | mdpi  | http://assets.emojidex.com/emoji/mdpi/blush.png
![blush emoji](http://assets.emojidex.com/emoji/hdpi/blush.png)  | hdpi  | http://assets.emojidex.com/emoji/hdpi/blush.png
![blush emoji](http://assets.emojidex.com/emoji/xhdpi/blush.png) | xhdpi | http://assets.emojidex.com/emoji/xhdpi/blush.png
![blush emoji](http://assets.emojidex.com/emoji/px8/blush.png)   | px8   | http://assets.emojidex.com/emoji/px8/blush.png
![blush emoji](http://assets.emojidex.com/emoji/px16/blush.png)  | px16  | http://assets.emojidex.com/emoji/px16/blush.png
![blush emoji](http://assets.emojidex.com/emoji/px32/blush.png)  | px32  | http://assets.emojidex.com/emoji/px32/blush.png
![blush emoji](http://assets.emojidex.com/emoji/px64/blush.png)  | px64  | http://assets.emojidex.com/emoji/px64/blush.png
![blush emoji](http://assets.emojidex.com/emoji/px128/blush.png) | px128 | http://assets.emojidex.com/emoji/px128/blush.png
![blush emoji](http://assets.emojidex.com/emoji/px256/blush.png) | px256 | http://assets.emojidex.com/emoji/px256/blush.png

<aside class="warning">
Only obtian the images you need only in the size that matches your use case or the nearest size 
that is larger than your use case.
</aside>
<aside class="warning">
Do not agressively download/cache emoji you may not be displaying. If you are going to pre-cache 
some emoji images please limit it to only a few (eg. 10 or less).
</aside>

In general small text will usually be best with an emoji 
size of about mdpi ![blush emoji](http://assets.emojidex.com/emoji/mdpi/blush.png). Regular text, 
as in text that is readable on most mobile handsets without zooming, will usually be around hdpi 
![blush emoji](http://assets.emojidex.com/emoji/hdpi/blush.png).

> Get the SVG image for :blush:  
```
http://assets.emojidex.com/emoji/blush.svg
```

> Get the 32px PNG image for :blush:  
```
http://assets.emojidex.com/emoji/px32/blush.png
```

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import 'kittn'

api = Kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/3"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Isis",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">If you're not using an administrator API key, note that some kittens will return 403 Forbidden if they are hidden for admins only.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the cat to retrieve

