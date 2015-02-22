---
language_tabs:
  - shell
  - ruby
  - javascript
  - java
---

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

## Authentication

> Key acquisition:

```shell
curl -X GET www.emojidex.com/api/v1/users/authenticate -d email=whoever@emojidex.com -d password=********
# or
curl -X GET www.emojidex.com/api/v1/users/authenticate -d username=WhoEver -d password=********
```

```ruby
# coming soon
```

```javascript
// Using the emojidex-web-client
emojidex = new EmojidexClient();
emojidex.User.login({'authtype': 'plain', 'username': 'MeMeMe', 'password': '******'});
```

```java
// coming soon
```

> When authentication succeeds the following JSON is returned:

```javascripton
{"auth_status":"verified","auth_token":"1234567890abcdef","auth_user":"MyUserName"}
```

> When authentication fails the follwing JSON is returned:

```javascripton
{"auth_status":"unverified","auth_token":null}
```

> Key utilization example:

```shell
curl -X GET https://www.emojidex.com/api/v1/users/favorites -d auth_token=1234567890abcdef
```

```ruby
```

```javascript
// the emojidex web client will automatically insert auth keys for you
```

```java
```

### API Keys / Auth Tokens
Each user is granted an auth token which gives them access to their own personal resources. 
Each user is granted one and only 
one API key. In the future app-specific keys will be issued along with auth provider independent 
login tokens, but for the time being one single API key is granted and used. This API key 
functionality will not be depricated in version 1 of the API, so there will be no need to 
adjust for the addition of app-specific keys or auth-specific tokens.  
<aside class="notice">
It should be noted that these resources do not include any personally identifying resources,
nor any resources that allow a user to make payments or change account details such as 
passwords. For Version 1 of the API, the worst that can happen from a stolen API Key is 
having favorites added or removed, or items added to a users history. Reguardless, writing code 
that attempts to mis-use API Keys is forbidden by the Terms of Use and may be met with legal 
action. Keep your users keys safe, and do not mis-use them!
</aside>

API Keys are returned as a response from a valid authorization request. Users can view, re-set, 
or revoke their individual API Key from the User Details section after they log in to the 
emojidex web site. An API key may be revoked from the User Settings, but not from the API. 
As a single API Key is granted per user and shared between apps, the revocation or 
re-generation of an API Key will require re-acquisition in all clients.
Not all API calls require authentication, but all API calls can and should be performed with 
authentication tokens included. Generally, an auth token should be used with every API call 
other than the initial authentication request.

### Authenticating / Obtaining user auth token
To authenticate you must add the "user" and "auth_token" fields to the request. The "user" field
is the user name, and the auth_token can be obtained with an authorize request.
<aside class="warning">
DO NOT store a users password. ALWAYS use an auth token. ONLY pass the password to the API when 
you are obtaining an auth token. Store auth tokens as securely as possible!
</aside>

## emoji Information
> Get information on the "sushi" emoji
> GET /emoji/sushi

```shell
curl -X GET https://www.emojidex.com/api/v1/emoji/sushi
```

```ruby
```

```javascript
```

```java
```

> Information about the "sushi" emoji is retruned:

```javascripton
{"code":"sushi","moji":"🍣","unicode":"1f363","category":"food","tags":[]}
```

To get information about a particular emoji simply query they /emoji/(emoji code) endpoint, 
replacing (emoji code) with the emoji code you want the information about.

*Parameters*

Name | Type | Description
---- | ---- | -----------
detailed | bool | returns extra information such as upstream asset checksums[md5] when true

## emoji Index

> GET /emoji

```shell
curl -X GET https://www.emojidex.com/api/v1/emoji
```

```ruby
```

```javascript
emojidex.Indexes.index();
```

```java
```

> GET /emoji detailed=true

```shell
curl -X GET https://www.emojidex.com/api/v1/emoji -d detailed=true
```

```ruby
```

```javascript
emojidex.Indexes.index(null, {'detailed': true});
```

```java
```

> GET /emoji limit=50 page=2

```shell
curl -X GET https://www.emojidex.com/api/v1/emoji -d page=2 -d limit=50
```

```ruby
```

```javascript
emojidex.Indexes.index(null, {'page': 2, 'limit': 50});
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
detailed | bool | returns extra information such as upstream asset checksums[md5] when true


## Newest emoji

> GET /newest

```shell
curl -X GET https://www.emojidex.com/api/v1/newest
```

```ruby
```

```javascript
emojidex.Indexes.newest();
```

```java
```

Gets a general index of emoji sorted by registration date.  

*Parameters*

Name | Type | Description
---- | ---- | -----------
limit | integer | amount of emoji to return per page
page | integer | page number
detailed | bool | returns extra information such as upstream asset checksums[md5] when true

## Popular emoji

> GET /popular

```shell
curl -X GET https://www.emojidex.com/api/v1/popular
```

```ruby
```

```javascript
emojidex.Indexes.popular();
```

```java
```

Gets a general index of emoji sorted by popularity.  

*Parameters*

Name | Type | Description
---- | ---- | -----------
limit | integer | amount of emoji to return per page
page | integer | page number
detailed | bool | returns extra information such as upstream asset checksums[md5] when true

Categories
----------

> GET /categories

```shell
curl -X GET https://www.emojidex.com/api/v1/categories
```

```ruby
```

```javascript
// categories should be pre-cached
emojidex.Categories.all()
// you can actively obtain categories from the server
emojidex.Categories.get();
```

```java
```

> GET /categories locale=ja

```shell
curl -X GET https://www.emojidex.com/api/v1/categories -d locale=ja
```

```ruby
```

```javascript
emojidex.Categories.get('ja');
```

```java
```

Gets a list of categoires, including category codes (used in searching) and [localized] titles.

## Search for emoji

> Search for emoji with code containing "heart"

```shell
curl -X GET https://www.emojidex.com/api/v1/search/emoji -d code_cont=heart
```

```javascript
emojidex.Search.search("heart");
```

> Search for emoji with code starting with "heart"

```shell
curl -X GET https://www.emojidex.com/api/v1/search/emoji -d code_sw=heart
```

```javascript
emojidex.Search.starting("heart");
```

> Search for emoji with code ending with "heart"

```shell
curl -X GET https://www.emojidex.com/api/v1/search/emoji -d code_ew=heart
```

```javascript
emojidex.Search.ending("heart");
```

> Search for emoji with codes containing "heart" in the category "faces"

```shell
curl -X GET https://www.emojidex.com/api/v1/search/emoji -d code_cont=heart -d categories\[\]=faces
```

```javascript
emojidex.Search.advanced("heart", [], ['faces']);
```

> Search for emoji with codes containing "heart" in either the category "faces" or "abstract"

```shell
curl -X GET https://www.emojidex.com/api/v1/search/emoji -d code_cont=heart -d categories\[\]=faces -d categories\[\]=abstract
```

```javascript
emojidex.Search.advanced("heart", [], ['faces', 'abstract']);
```

> Search for emoji with tag "weapon" 

```shell
curl -X GET https://www.emojidex.com/api/v1/search/emoji -d tags\[\]=weapon
```

```javascript
emojidex.Search.tags(["weapon"]);
```

> Search for emoji where code contains "rifle", has the tag "weapon", and is in category "tools"

```shell
curl -X GET https://www.emojidex.com/api/v1/search/emoji -d code_cont=rifle -d tags\[\]=weapon -d categories\[\]=tools
```

```javascript
emojidex.Search.advanced("rifle", ['weapon'], ['tools']);
```

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

*Predicates*
Predicates are added to the code field to specify the search type, such as "\[q\]\[code_cont\]" 
to "query for codes which contain" the specified term.

Predicate | Meaning | Description
---- | ---- | -----------
cont | Contains | Contains the search term somewhere in the code.
sw | Starts With | Code starts with the term.
ew | Ends With | Code ends with the term.

<aside class="notice">
There is no "exact match" search because simply hitting https://www.emojidex.com/api/v1/emoji/#code 
will achieve the same thing - it directly checks for the emoji with that code (replacing #code with 
the code you are checking for).
</aside>

*Search Fields*

Name | Description
---- | -----------
code | Code search. Can use predicates.
tags[] | An array of tags to search by or to limit results to.
categories[] | An array of categories to limit the search domain.

*Parameters*

Name | Type | Description
---- | ---- | -----------
limit | integer | amount of emoji to return per page
page | integer | page number
detailed | bool | returns extra information such as upstream asset checksums[md5] when true

## User emoji

> Get emoji for the user "Zero"

```shell
curl -X GET https://www.emojidex.com/api/v1/users/Zero/emoji
```

```javascript
emojidex.Index.user("Zero");
```

> Get emoji for the user "絵文字"

```shell
curl -X GET https://www.emojidex.com/api/v1/users/絵文字/emoji
```

```javascript
emojidex.Index.user("絵文字");
```

You can get all emoji registered by a specific user.  
Some special user accounts exist from which you can get the current UTF 
index, emojidex original extended emoji, etc. These accounts are as 
follows:

  * emoji: All UTF/Unicode emoji with English emoji codes
  * emojidex: All emojidex original extended emoji with English emoji codes
  * 絵文字: UTF/Unicode emoji with Japanese emoji codes
  * 絵文字デックス: emojidex original extended emoji with Japanese emoji codes

*Parameters*

Name | Type | Description
---- | ---- | -----------
limit | integer | amount of emoji to return per page
page | integer | page number
detailed | bool | returns extra information such as upstream asset checksums[md5] when true

## Favorites

Favorites can only be accessed with a token. This resource requires authentication.

*Parameters*

Name | Type | Description
---- | ---- | -----------
limit | integer | amount of emoji to return per page
page | integer | page number
detailed | bool | returns extra information such as upstream asset checksums[md5] when true

> Get favorites:
```shell
curl -X GET https://www.emojidex.com/api/v1/users/favorites -d auth_token=1234567890abcdef
```

```javascript
// favorites should be pre-cached on login/initialization
emojidex.User.Favorites.all()
// you can actively obtain favorites from the server
emojidex.User.Favorites.get();
```

> Add an emoji to favorites:
```shell
curl -X POST https://www.emojidex.com/api/v1/users/favorites -d auth_token=1234567890abcdef -d emoji_code=zebra
```

```javascript
emojidex.User.Favorites.set("zebra");
```

> Remove an emoji from favorites:
```shell
curl -X DELETE https://www.emojidex.com/api/v1/users/favorites -d auth_token=1234567890abcdef -d emoji_code=zebra
```

```javascript
emojidex.User.Favorites.unset("zebra");
```

## History

History can only be accessed with a token. This resource requires authentication.

*Parameters*

Name | Type | Description
---- | ---- | -----------
limit | integer | amount of emoji to return per page
page | integer | page number
detailed | bool | returns extra information such as upstream asset checksums[md5] when true

> Get history:

```shell
curl -X GET https://www.emojidex.com/api/v1/users/history -d auth_token=1234567890abcdef
```

```javascript
// history is pre-cached on login/initialization
emojidex.User.History.all()
// you can active obtain history from the server
emojidex.User.History.get();
```

> Add an emoji to history:
```shell
curl -X POST https://www.emojidex.com/api/v1/users/history -d auth_token=1234567890abcdef -d emoji_code=zebra
```

```javascript
emoji.User.History.set("zebra");
```
