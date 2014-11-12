# API
<aside class="notice">
This documenation is for Version 1 of the API.
</aside>

This documentation covers access and usage of emojidex service utilizing the API and CDN.

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

> Token acquisition:

```ruby
```

```js
```

```shell
curl www.emojidex.com/api/v1
```

> Token utilization:

```ruby
```

```js
```

```shell
curl www.emojidex.com/api/v1
```

Not all API calls require authentication, but all API calls can be performed with authentication 
tokens included. Generally if a user has an auth token it should be used with every call.

To authenticate you must add the "user" and "auth_token" fields to the request. The "user" field
is the user name, and the auth_token can be obtained with an authorize request.
<aside class="warning">
DO NOT store a users password. ALWAYS use an auth token. ONLY pass the password to the API when 
you are obtaining an auth token. Store auth tokens as securely as possible!
</aside>

### Obtain a Token

TODO


### Using a Token
TODO

## emoji Index

> GET /emoji

```shell
curl -X GET https://www.emojidex.com/api/v1/emoji
```

```ruby
```

```js
```

```java
```

> GET /emoji limit=50 page=2

```shell
curl -X GET https://www.emojidex.com/api/v1/emoji -d page=2 -d limit=50
```

```ruby
```

```js
```

```java
```

Gets a general index of emoji.  
<aside class="notice">
This index is ordered by a combination of registration date and popularity. 
Newer and more popular emoji will show up first.
</aside>

*Parameters*

Name | Type | Description
---- | ---- | -----------
limit | integer | amount of emoji to return per page
page | integer | page number

## Detailed emoji index

> GET /emoji/detailed

```shell
curl -X GET https://www.emojidex.com/api/v1/emoji/detailed
```

```ruby
```

```js
```

```java
```

> GET /emoji/detailed limit=50 page=2

```shell
curl -X GET https://www.emojidex.com/api/v1/emoji/detailed -d page=2 -d limit=50
```

```ruby
```

```js
```

```java
```

Gets a detailed index of emoji, wherein each emoji entry contains all the extended 
information associated with that emoji.
<aside class="notice">
This index is ordered by a combination of registration date and popularity. 
Newer and more popular emoji will show up first.
</aside>

*Parameters*

Name | Type | Description
---- | ---- | -----------
limit | integer | amount of emoji to return per page
page | integer | page number

## Newest emoji

> GET /newest

```shell
curl -X GET https://www.emojidex.com/api/v1/newest
```

```ruby
```

```js
```

```java
```

> GET /emoji limit=50 page=2

```shell
curl -X GET https://www.emojidex.com/api/v1/newest -d page=2 -d limit=50
```

```ruby
```

```js
```

```java
```

Gets a general index of emoji sorted by registration date.  

*Parameters*

Name | Type | Description
---- | ---- | -----------
limit | integer | amount of emoji to return per page
page | integer | page number

## Popular emoji

> GET /popular

```shell
curl -X GET https://www.emojidex.com/api/v1/popular
```

```ruby
```

```js
```

```java
```

> GET /emoji limit=50 page=2

```shell
curl -X GET https://www.emojidex.com/api/v1/popular -d page=2 -d limit=50
```

```ruby
```

```js
```

```java
```

Gets a general index of emoji sorted by popularity.  

*Parameters*

Name | Type | Description
---- | ---- | -----------
limit | integer | amount of emoji to return per page
page | integer | page number

## Search for emoji
The basis for emoji searches is search by emoji code. Searches can be performed for 
codes that contain a term, start with a term, or end with a term. Alternatively, 
you can serch for tags, or you can combine a code search with tags to restrict search 
results to emoji containging the term which also have the tags specified. Additionally, 
searches can be restricted to one or more categories, where results will only be emoji 
present in the categories specified. In general, more tags specified means a more 
specific search with fewer results. And, in general, the more categories specified the 
more generalized the search and therefore more results. When no categories or tags are 
specified the search will not be restricted to any categories or tags and will return 
all results for the term specified.

The API endpoint for searches is /search/emoji.

*Prefix*
All advanced search fields have a [q] prefix. The [q] prefix simply makes the distinction 
that the argument being passed is a query term and not a control method or other argument.

*Predicates*
Predicates are added to the code field to specify the search type, such as "\[q\]\[code_cont\]" 
to "query for codes which contain" the specified term.

Predicate | Meaning | Description
---- | ---- | -----------
cont | Contains | Contains the search term somewhere in the code.
eq | Equals | Is exactly equal to the term given. This parameter will nullify any other terms.
sw | Starts With | Code starts with the term.
ew | Ends With | Code ends with the term.

<aside class="notice">
All predicates other than "eq" can be mixed.
</aside>

*Search Fields*

Name | Description
---- | -----------
code | Code search. Can use predicates.
tags | An array of tags to search by or to limit results to.
categories | An array of categories to limit the search domain.

*Parameters*

Name | Type | Description
---- | ---- | -----------
limit | integer | amount of emoji to return per page
page | integer | page number

> Search for emoji with code containing "heart"

```shell
curl -X GET https://www.emojidex.com/api/v1/search/emoji -d \[q\]\[code_cont\]=heart
```

> Search for emoji with code starting with "heart"

```shell
curl -X GET https://www.emojidex.com/api/v1/search/emoji -d \[q\]\[code_sw\]=heart
```

> Search for emoji with code ending with "heart"

```shell
curl -X GET https://www.emojidex.com/api/v1/search/emoji -d \[q\]\[code_ew\]=heart
```

> Search for emoji with the exact code of "heart"

```shell
curl -X GET https://www.emojidex.com/api/v1/search/emoji -d \[q\]\[code_eq\]=heart
```

> Search for emoji with codes containing "heart" in the category "faces"

```shell
curl -X GET https://www.emojidex.com/api/v1/search/emoji -d \[q\]\[code_cont\]=heart -d \[q\]\[categories\]=\[faces\]
```

> Search for emoji with codes containing "heart" in either the category "faces" or "abstract"

```shell
curl -X GET https://www.emojidex.com/api/v1/search/emoji -d \[q\]\[code_cont\]=heart -d \[q\]\[categories\]=\[faces,abstract\]
```

> Search for emoji with tag "weapon" 

```shell
curl -X GET https://www.emojidex.com/api/v1/search/emoji -d \[q\]\[tags\]=\[weapon\]
```

> Search for emoji where code contains "rifle", has the tag "weapon", and is in category "tools"

```shell
curl -X GET https://www.emojidex.com/api/v1/search/emoji -d \[q\]\[code_cont\]=rifle -d \[q\]\[tags\]=\[weapon\] -d \[q\]\[categories\]=\[tools\]
```

## User emoji

> Get emoji for the user "Zero"

```shell
curl -X GET https://www.emojidex.com/api/v1/users/Zero/emoji
```

> Get emoji for the user "絵文字"

```shell
curl -X GET https://www.emojidex.com/api/v1/users/絵文字/emoji
```

You can get all emoji registered by a specific user.  
Some special user accounts exist from which you can get the current UTF 
index, emojidex original extended emoji, etc. These accounts are as 
follows:

  * emoji: All UTF/Unicode emoji with English emoji codes
  * emojidex: All emojidex original extended emoji with English emoji codes
  * 絵文字: UTF/Unicode emoji with Japanese emoji codes
  * 絵文字デックス: emojidex original extended emoji with Japanese emoji codes

