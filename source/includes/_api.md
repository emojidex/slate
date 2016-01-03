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

## HTTP Verbs
Throughout most of the API different actions are mapped to different HTTP verbs, such as GET, 
PUT, POST, DELETE. You MUST be aware of what verb you are using to make an API request, as most 
of the time only one HTTP verb will perform the task you want in your query.

## Authentication

> Token acquisition:

```shell
curl -X GET www.emojidex.com/api/v1/users/authenticate -d email=whoever@emojidex.com -d password=********
# or
curl -X GET www.emojidex.com/api/v1/users/authenticate -d username=WhoEver -d password=********
# or
curl -X GET www.emojidex.com/api/v1/users/authenticate -d user=whoever@emojidex.com -d password=********
# or
curl -X GET www.emojidex.com/api/v1/users/authenticate -d user=WhoEver -d password=********
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

> Token confirmation:

```shell
curl -X GET www.emojidex.com/api/v1/users/authenticate -d username=MeMeMe -d token=0123456789abcdef
```

```ruby
# coming soon
```

```javascript
// Using the emojidex-web-client
// coming soon
```

```java
// coming soon
```

> When authentication / confirmation succeeds something like the following JSON is returned:

```json
{"auth_status":"verified","auth_user":"MyUserName","auth_token":"0123456789abcdef",
"pro":false,"pro_exp":null,"premium":true,"premium_exp":1467431422, "r18":false}
```

> When authentication fails the follwing JSON is returned:

```json
{"auth_status":"unverified","auth_token":null}
```

> Token utilization example:

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

### Auth Tokens
Each user is granted an auth token which gives them access to their own personal resources. 
Each user account is granted one and only one token which can be used by multiple clients, 
does not expire, but can be refreshed/changed.  
  
<aside class="notice">
The tokens issued and used in V1 of the API are "level 1" tokens. 
They do not grant access to any resources which contain personally identifying information,
nor any resources that allow a user to make payments or change account details such as 
passwords. The worst that can happen from a stolen token is having favorites added or removed, 
or items added to a users history.
</aside>

<aside class="warning">
Utilizing auth tokens to add or remove favorites or add to history without a users consent may 
violate usage licenses of emojidex. emojidex may take legal action against anyone found abusing 
access to users auth tokens without first notifying the user and/or obtaining their consent.
</aside>

Tokens are returned as a response from a valid authorization request. Users can view, re-set, 
or revoke their auth token from the User Details section after they log in to the 
emojidex web site. Auth tokens can not be changed through the API. 
As a single auth token is granted per user and shared between apps, the revocation or 
re-generation of a token will require re-acquisition in all clients.
Not all API calls require authentication, but all API calls can and should be performed with 
authentication tokens included when a token is available. 

### Authenticating / Obtaining a user auth token from the API
You can authenticate through the API using a username and password or e-mail and password. 
It is also possible to check authentication and update user details using the username and token. 
To authenticate you must make a call to /api/v1/users/authenticate with an appropriate 
combination of the following parameters:

*Parameters*

Name | Type | Description
---- | ---- | -----------
username | string | The username registered to the user (paired with password or token)
email | string | The e-mail address of the user (paired with password)
user | string | The username or e-mail address of the user (paired with password)
password | string | The password of the user (paired with username or e-mail)
token | string | The authentication token (paired with username)

<aside class="notice">
There is no inherent demerit to using "user" rather than "username" or "email", but we 
generally recommend using a username as it is not directly personally identifying and 
it is the only part of a user account that can not be changed and remains static.
</aside>

Upon a successful authentication request the following information will be returned:

*Return Data*

┏auth_status: the status code of the authorization ("verified" or "unverified")  
┣auth_user: the username of the account just verified  
┣auth_token: the auth token for this account  
┣pro: true or false value specifying if pro status is currently active  
┣pro_exp: expiration date of current or previous pro status (null if never active)  
┣premium: true or false value specifying if premium status is currently active  
┣premium_exp: expiration date of current or previous premium status (null if never active)  
┗r18: true or false value specifying if user wants to view R-18 content  

<aside class="warning">
DO NOT store a users password. ALWAYS use an auth token. ONLY pass the password to the API when 
you are obtaining an auth token. Store auth tokens as securely as possible!
</aside>

## emoji Seeds
> Get all UTF [standrd] emoji

```shell
curl -X GET https://www.emojidex.com/api/v1/utf_emoji
```

> Get all emojidex brand Extended emoji

```shell
curl -X GET https://www.emojidex.com/api/v1/extended_emoji
```

> Get all UTF [standard] emoji with Japanese codes

```shell
curl -X GET https://www.emojidex.com/api/v1/utf_emoji -d locale=ja
```

> Get all emojidex brand Extended emoji with Japanese codes

```shell
curl -X GET https://www.emojidex.com/api/v1/extended_emoji -d locale=ja
```

> Get moji code indexes

```shell
curl -X GET https://www.emojidex.com/api/v1/moji_codes
```

> The returned data contains three major indexes.

```json
/* ... indicates truncation, the actual data does not have a ... */
{
  /* useful for REGEX checking/filtering */
  "moji_string":"⛲⛳⛺⛽✅✉✌〽️...",
  /* useful for manual scanning */
  "moji_array":["️1️⃣","️2️⃣","️3️⃣","️4️⃣","️5️⃣","️6️⃣","️7️⃣","️8️⃣", ...],
  /* useful for mapping characters to short codes or for a very basic index */
  "moji_index":{"⛲":"fountain","⛳":"golf","⛺":"tent","⛽":"fuelpump", ...}
}
```

> Get moji code indexes with Japanese codes

```shell
curl -X GET https://www.emojidex.com/api/v1/moji_codes -d locale=ja
```

Generally you'll want to have a selection of emoji available immediately. Usually that will at least 
consist of the standardized UTF/Unicode emoji. A semi-static index is available at api/v1/utf_emoji .
If you only need the codes, a combined string for regex filtering, or an index of character code to 
emoji short code, these all come in the data available from api/v1/moji_codes . The emojidex branded 
extended emoji are also available from api/v1/extended_emoji. All of these end points are compatible 
with the locale option.

<aside class="notice">
All these calls give very long JSON responses.
</aside>

*Parameters*

Name | Type | Description
---- | ---- | -----------
locale | string | the locale/languge code (EG: "ja" or "en")

<aside class="notice">
All emoji details from seeds contain all detailed information. There is no enabling or disabling 
extended details with these calls.
</aside>

*Return Data*

┏moji_string: a string of character codes, with multi-chracter compound codes coming before others  
┣moji_array[]: an array of character codes, with multi-chracter compound codes coming before others  
┗moji_index{}: a localized hash index of character code keys with emoji code values  

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

```json
{"code":"sushi","moji":"🍣","unicode":"1f363","category":"food","tags":[]}
```

To get information about a particular emoji simply query they /emoji/(emoji code) endpoint, 
replacing (emoji code) with the emoji code you want the information about.

*Parameters*

Name | Type | Description
---- | ---- | -----------
detailed | bool | returns extra information such as upstream asset checksums[md5] when true

*Return Data*

┏code: the :code: of the emoji  
┣moji: the character code of the emoji (null when emoji is not a standard character)  
┣unicode: the unicode ID of the emoji (null when emoji is not a standard character)  
┣category: the category the emoji is contained in  
┣tags[]: an array of tags the emoji is registered under (an empty array [] if none)  
┣link: a URL associated with the emoji (null when no link is registered or active)  
┣base: the code of the emoji this emoji is based off of (code of this emoji if not a variant)  
┣variants[]: an array of variants of this emoji  
┗score: the score of the emoji  

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

Gets a general index of emoji, sorted by score.  
<aside class="notice">
This index is ordered by a combination of registration date and popularity. 
Newer and more popular emoji will show up first.
</aside>

*Parameters*

Name | Type | Description
---- | ---- | -----------
category | string | the category code (EG: "faces" or "food") you wish to limit the index to
limit | integer | amount of emoji to return per page
page | integer | page number
detailed | bool | returns extra information such as upstream asset checksums[md5] when true

*Return Data*

┏emoji[]: an array of emoji  
┃┣code: the :code: of the emoji  
┃┣moji: the character code of the emoji (null when emoji is not a standard character)  
┃┣unicode: the unicode ID of the emoji (null when emoji is not a standard character)  
┃┣category: the category the emoji is contained in  
┃┣tags[]: an array of tags the emoji is registered under (an empty array [] if none)  
┃┣link: a URL associated with the emoji (null when no link is registered or active)  
┃┣base: the code of the emoji this emoji is based off of (code of this emoji if not a variant)  
┃┣variants[]: an array of variants of this emoji  
┃┗score: the score of the emoji  
┗meta{}: meta data about this collection  
　┣count: the count of emoji [limit] contained in this response  
　┣total_count: the total count emoji in this collection available on the server  
　┗page: the current page of the collection  

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

<aside class="notice">
This resource can only be accessed by pro or premium users.
</aside>

*Parameters*

Name | Type | Description
---- | ---- | -----------
auth_token | string | a user auth token. User must be pro or premium to access this resource
category | string | the category code (EG: "faces" or "food") you wish to limit the index to
limit | integer | amount of emoji to return per page
page | integer | page number
detailed | bool | returns extra information such as upstream asset checksums[md5] when true

HTTP | Message
---- | -------
200  | a page of an emoji collection
401  | {"status":"resource requires authorization"}
402  | {"status":"resource limited to premium/pro users"}

*Return Data*

┏emoji[]: an array of emoji  
┃┣code: the :code: of the emoji  
┃┣moji: the character code of the emoji (null when emoji is not a standard character)  
┃┣unicode: the unicode ID of the emoji (null when emoji is not a standard character)  
┃┣category: the category the emoji is contained in  
┃┣tags[]: an array of tags the emoji is registered under (an empty array [] if none)  
┃┣link: a URL associated with the emoji (null when no link is registered or active)  
┃┣base: the code of the emoji this emoji is based off of (code of this emoji if not a variant)  
┃┣variants[]: an array of variants of this emoji  
┃┗score: the score of the emoji  
┗meta{}: meta data about this collection  
　┣count: the count of emoji [limit] contained in this response  
　┣total_count: the total count emoji in this collection available on the server  
　┗page: the current page of the collection  

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

Gets a general index of emoji sorted by popularity (times favorited).  

<aside class="notice">
This resource can only be accessed by pro or premium users.
</aside>

*Parameters*

Name | Type | Description
---- | ---- | -----------
auth_token | string | a user auth token. User must be pro or premium to access this resource
category | string | the category code (EG: "faces" or "food") you wish to limit the index to
limit | integer | amount of emoji to return per page
page | integer | page number
detailed | bool | returns extra information such as upstream asset checksums[md5] when true

*Return Status*

HTTP | Message
---- | -------
200  | a page of an emoji collection
401  | {"status":"resource requires authorization"}
402  | {"status":"resource limited to premium/pro users"}

*Return Data*

┏emoji{}: an array of emoji  
┃┣code: the :code: of the emoji  
┃┣moji: the character code of the emoji (null when emoji is not a standard character)  
┃┣unicode: the unicode ID of the emoji (null when emoji is not a standard character)  
┃┣category: the category the emoji is contained in  
┃┣tags[]: an array of tags the emoji is registered under (an empty array [] if none)  
┃┣link: a URL associated with the emoji (null when no link is registered or active)  
┃┣base: the code of the emoji this emoji is based off of (code of this emoji if not a variant)  
┃┣variants[]: an array of variants of this emoji  
┃┗score: the score of the emoji  
┗meta{}: meta data about this collection  
　┣count: the count of emoji [limit] contained in this response  
　┣total_count: the total count emoji in this collection available on the server  
　┗page: the current page of the collection  

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

> Returns category info

```json
{"categories":[{"name":"Abstract","code":"abstract","emoji_count":967},
{"name":"Cosmos","code":"cosmos","emoji_count":110},
{"name":"Faces","code":"faces","emoji_count":1702},
{"name":"Food","code":"food","emoji_count":448},
{"name":"Gestures","code":"gestures","emoji_count":657},
{"name":"Nature","code":"nature","emoji_count":833},
{"name":"Objects","code":"objects","emoji_count":1848},
{"name":"People","code":"people","emoji_count":997},
{"name":"Places","code":"places","emoji_count":163},
{"name":"Symbols","code":"symbols","emoji_count":1572},
{"name":"Tools","code":"tools","emoji_count":79},
{"name":"Transportation","code":"transportation","emoji_count":277}],
"meta":{"count":12,"total_count":12,"page":1}}
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

> Returns category info with Japanese names

```json
{"categories":[{"name":"抽象物","code":"abstract","emoji_count":967},
{"name":"宇宙","code":"cosmos","emoji_count":110},
{"name":"顔","code":"faces","emoji_count":1702},
{"name":"食べ物","code":"food","emoji_count":448},
{"name":"ジェスチャー","code":"gestures","emoji_count":657},
{"name":"自然","code":"nature","emoji_count":833},
{"name":"物","code":"objects","emoji_count":1848},
{"name":"人","code":"people","emoji_count":997},
{"name":"場所","code":"places","emoji_count":163},
{"name":"記号","code":"symbols","emoji_count":1572},
{"name":"道具","code":"tools","emoji_count":79},
{"name":"乗り物","code":"transportation","emoji_count":277}],
"meta":{"count":12,"total_count":12,"page":1}}
```

Gets a list of categoires, including category codes (used in searching) and [localized] titles.

<aside class="notice">
While the categories endpoint can take limit and page parameters, there are currently not enough 
categories that you'd need to use them. The defaults will give you a full list of categories.
</aside>

*Parameters*

Name | Type | Description
---- | ---- | -----------
limit | integer | amount of categories to return per page
page | integer | page number
locale | string | language for category names currently only "en" [default] and "ja" are supported

*Return Data*

┏categories[]: an array of categories  
┃┣name: the localized category name  
┃┣code: the category code  
┃┗emoji_count: the number of emoji currently in this category  
┗meta{}: meta data about the array of categories  
　┣count: the number of categories in this response  
　┣total_count: the total count of categories available on the server  
　┗page: the current page of categories  

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

*Return Data*

┏emoji[]: an array of emoji  
┃┣code: the :code: of the emoji  
┃┣moji: the character code of the emoji (null when emoji is not a standard character)  
┃┣unicode: the unicode ID of the emoji (null when emoji is not a standard character)  
┃┣category: the category the emoji is contained in  
┃┣tags[]: an array of tags the emoji is registered under (an empty array [] if none)  
┃┣link: a URL associated with the emoji (null when no link is registered or active)  
┃┣base: the code of the emoji this emoji is based off of (code of this emoji if not a variant)  
┃┣variants[]: an array of variants of this emoji  
┃┗score: the score of the emoji  
┗meta{}: meta data about this collection  
　┣count: the count of emoji [limit] contained in this response  
　┣total_count: the total count emoji in this collection available on the server  
　┗page: the current page of the collection  

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

> Returns a collection

```json
{"emoji":[{"code":"幻","moji":null,"unicode":null,"category":"abstract","tags":[],"link":null,
"base":"幻","variants":["幻","幻(白)"],"score":0},{"code":"pink poo","moji":null,
"unicode":null,"category":"abstract","tags":[],"link":null,"base":"pink_poo",
"variants":["pink_poo"],"score":340}],"meta":{"count":2,"total_count":2,"page":1}}
```

You can get all emoji registered by a specific user.  
Some special user accounts exist from which you can get the current UTF 
index, emojidex original extended emoji, etc. These accounts are as 
follows:

  * emoji: All UTF/Unicode emoji with English emoji codes
  * emojidex: All emojidex original extended emoji with English emoji codes
  * 絵文字: UTF/Unicode emoji with Japanese emoji codes
  * 絵文字デックス: emojidex original extended emoji with Japanese emoji codes

However, this information can also be obtained from the emoji Seed paths in one 
call without pagination (see above).

*Parameters*

Name | Type | Description
---- | ---- | -----------
limit | integer | amount of emoji to return per page
page | integer | page number
detailed | bool | returns extra information such as upstream asset checksums[md5] when true

*Return Data*

┏emoji[]: an array of emoji  
┃┣code: the :code: of the emoji  
┃┣moji: the character code of the emoji (null when emoji is not a standard character)  
┃┣unicode: the unicode ID of the emoji (null when emoji is not a standard character)  
┃┣category: the category the emoji is contained in  
┃┣tags[]: an array of tags the emoji is registered under (an empty array [] if none)  
┃┣link: a URL associated with the emoji (null when no link is registered or active)  
┃┣base: the code of the emoji this emoji is based off of (code of this emoji if not a variant)  
┃┣variants[]: an array of variants of this emoji  
┃┗score: the score of the emoji  
┗meta{}: meta data about this collection  
　┣count: the count of emoji [limit] contained in this response  
　┣total_count: the total count emoji in this collection available on the server  
　┗page: the current page of the collection  

## Favorites

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

```json
{"emoji":[{"code":"falafel","moji":null,"unicode":null,"category":"food","tags":[],"link":null,
"base":"falafel","variants":["falafel"],"score":100}],
"meta":{"count":1,"total_count":1,"page":1}}
```

> Add an emoji to favorites:

```shell
curl -X POST https://www.emojidex.com/api/v1/users/favorites -d auth_token=1234567890abcdef -d emoji_code=zebra
```

```javascript
emojidex.User.Favorites.set("zebra");
```

```json
{"code":"falafel","moji":null,"unicode":null,"category":"food","tags":[],"link":null,
"base":"falafel","variants":["falafel"],"score":106}
```

> Remove an emoji from favorites:

```shell
curl -X DELETE https://www.emojidex.com/api/v1/users/favorites -d auth_token=1234567890abcdef -d emoji_code=zebra
```

```javascript
emojidex.User.Favorites.unset("zebra");
```

```json
{"code":"falafel","moji":null,"unicode":null,"category":"food","tags":[],"link":null,
"base":"falafel","variants":["falafel"],"score":6}
```

Favorites can only be accessed with a token. This resource requires authentication.

**Obtaining favorites:**  
You can obtain favorites by performing a GET on the users/favorites endpoint with a valid 
auth_token.

*Parameters*

Name | Type | Description
---- | ---- | -----------
limit | integer | amount of emoji to return per page
page | integer | page number
detailed | bool | returns extra information such as upstream asset checksums[md5] when true

*Return Status*

HTTP | Message
---- | -------
200  | an array of user favorites
401  | {"status":"wrong authentication token"}

*Return Data*

┏emoji[]: an array of emoji  
┃┣code: the :code: of the emoji  
┃┣moji: the character code of the emoji (null when emoji is not a standard character)  
┃┣unicode: the unicode ID of the emoji (null when emoji is not a standard character)  
┃┣category: the category the emoji is contained in  
┃┣tags[]: an array of tags the emoji is registered under (an empty array [] if none)  
┃┣link: a URL associated with the emoji (null when no link is registered or active)  
┃┣base: the code of the emoji this emoji is based off of (code of this emoji if not a variant)  
┃┣variants[]: an array of variants of this emoji  
┃┗score: the score of the emoji  
┗meta{}: meta data about this collection  
　┣count: the count of emoji [limit] contained in this response  
　┣total_count: the total count emoji in this collection available on the server  
　┗page: the current page of the collection  

**Adding a Favorite:**  
You can add an emoji to a users favorites by performing a POST against the users/favorites 
endpoint with a valid auth_token and the emoji_code.

When a favorite is successfully added an HTTP return code of 201 with updated emoji information 
in the message body.

To prevent adding multiple entries of the same emoji an HTTP status of 202 with a JSON string 
{"status":"emoji already in user favorites"} will be returned. Check either the HTTP stats code 
or for this string so you don't accidentally duplicate the entry in your data set.

*Parameters*

Name | Type | Description
---- | ---- | -----------
auth_token | string | the auth token of the user whos favorites you wish to append to
emoji_code | string | the code of the emoji you wish to append to the users favorites

*Return Status*

HTTP | Message
---- | -------
201  | up to date details for the given emoji
202  | {"status":"emoji already in user favorites"}
401  | {"status":"wrong authentication token"}

*Return Data*

┏code: the :code: of the emoji  
┣moji: the character code of the emoji (null when emoji is not a standard character)  
┣unicode: the unicode ID of the emoji (null when emoji is not a standard character)  
┣category: the category the emoji is contained in  
┣tags[]: an array of tags the emoji is registered under (an empty array [] if none)  
┣link: a URL associated with the emoji (null when no link is registered or active)  
┣base: the code of the emoji this emoji is based off of (code of this emoji if not a variant)  
┣variants[]: an array of variants of this emoji  
┗score: the score of the emoji  

**Removing a Favorite**
You can remove an emoji from a users favorites by performing a DELETE against the users/favorites 
endpoint with a valid auth_token and the emoji_code.

When a favorite is successfully removed an HTTP return code of 200 with the emoji information 
in the body will be returned. You can use this information to cross check your local data and 
remove the emoji from the data set [though this is really only usefull if you have asyncronous 
or event driven code].

When a favorite is already remove an HTTP status of 202 with a JSON string 
{"status":"emoji not in user favorites"} will be returned.

*Parameters*

Name | Type | Description
---- | ---- | -----------
auth_token | string | the auth token of the user whos favorites you wish to remove from
emoji_code | string | the code of the emoji you wish to remove from the users favorites

*Return Status*

HTTP | Message
---- | -------
201  | up to date details for the given emoji
202  | {"status":"emoji not in user favorites"}
401  | {"status":"wrong authentication token"}

*Return Data*

┏code: the :code: of the emoji  
┣moji: the character code of the emoji (null when emoji is not a standard character)  
┣unicode: the unicode ID of the emoji (null when emoji is not a standard character)  
┣category: the category the emoji is contained in  
┣tags[]: an array of tags the emoji is registered under (an empty array [] if none)  
┣link: a URL associated with the emoji (null when no link is registered or active)  
┣base: the code of the emoji this emoji is based off of (code of this emoji if not a variant)  
┣variants[]: an array of variants of this emoji  
┗score: the score of the emoji  

## History

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

> A successful request will return a history array and meta info.

```json
{"history":[{"emoji_code":"falafel","times_used":6,"last_used":"2015-12-21T14:16:58.603+00:00"},
{"emoji_code":"sushi","times_used":4,"last_used":"2015-12-21T14:16:49.708+00:00"}],
"meta":{"count":2,"total_count":2,"page":1}}
```

> Add an emoji to history:

```shell
curl -X POST https://www.emojidex.com/api/v1/users/history -d auth_token=1234567890abcdef -d emoji_code=zebra
```

```javascript
emoji.User.History.set("zebra");
```

> A successful registration will return the updated history entry:

```json
{"emoji_code":"zebra","times_used":1,"last_used":"2015-11-30T04:18:23.647+00:00"}
```

History can only be accessed with a token. This resource requires authentication.

**Obtaining history:**  
You can obtain history by performing a GET on the users/history endpoint with a valid auth_token.

<aside class="notice">
The history is NOT a standard collection. Do not attempt to parse it like an "emoji" array returned 
by most other calls.
</aside>

*Parameters*

Name | Type | Description
---- | ---- | -----------
auth_token | string | the auth token of the user whos history you wish to obtain
limit | integer | amount of emoji to return per page
page | integer | page number


*Return Status*

HTTP | Message
---- | -------
201  | an array containing emoji codes and times and dates used
401  | {"status":"wrong authentication token"}


*Return Data*

┏history[]: an array containing emoji codes and times and dates used  
┃┣emoji_code: the :code: of the emoji  
┃┣times_used: the total number of times used by this users  
┃┗last_used: the :code: of the emoji  
┗meta{}: meta data about the history array  
　┣count: the count of history items [limit] contained in this response  
　┣total_count: the total count of entries in this users history  
　┗page: the current page of history  

**Adding to history:**  
Whenever a user uses an emoji you should append to their history by performing a POST on the 
users/history endpoint with the auth_token of the user and the emoji_code of the emoji being 
added.

*Parameters*

Name | Type | Description
---- | ---- | -----------
auth_token | string | the auth token of the user whos history you wish to append to
emoji_code | string | the code of the emoji you wish to append to the users history

*Return Status*

HTTP | Message
---- | -------
200  | an updated history entry for only the emoji appended
401  | {"status":"wrong authentication token"}
422  | {"status":"invalid emoji code"}

*Return Data*

┏emoji_code: the :code: of the emoji  
┣times_used: the total number of times used by this users  
┗last_used: the :code: of the emoji  
