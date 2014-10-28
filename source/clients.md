---
title: emojidex Client Libraries 

language_tabs:
  - ruby
  - java
  - js

toc_footers:
  - <a href='/'>API Documentation</a>
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>

search: true
---

# Client Module Overview

emojidex tools are generally developed with the same composition. Each Module implements a
Collection class which is used to store and access all the emoji information. Extensions to 
this class may enables acquisition of assets and caching when appropriate. Different modules 
may also come packaged with front end tools that automatically implement text areas with emoji 
automatically embedded, input tools such as soft keboards and searching etc. As a general 
guideline to where emojidex modules or libraries fit in your application we made this chart 
... entirely out of emoji.
![service to module to app](service-module-app.svg)

* Assets are stored on the content delivery network at assets.emojidex.com.
* The API is accessed from the core emojidex site.
* The emojidex client modules attempt to take advantage of this in the most efficient manner 
possible.
* Instead of packaging the base UTF and Extended emoji into each app and keeping a separate 
cache for each app, the emojidex client modules keep a common cache that also stores user 
information, favorites, and history in parsable formats (!some user information is encrypted).
* Simply including a client module into your app will give your users access to all the emoji 
they use without having to re-log-in or grant permissions to your app.
* In most client modules you have access to tools such as "emojify", which converts blocks of 
text into blocks of text with emoji images and assets embdedded.
* Some client modules are split into a library and a front end client/tools. The front end client 
and tools are [optionally] installed by the user and should not be bundled with your application. 
Instead, include the libary to enable automatic acquisition and display of emoji, and let the user 
install emojidex clients/tools themselves if they want to input / search / editors etc.

# Collections

Tools handle emoji in collections. Whenever possible all the emoji the user has used or viewed are 
kept in a local collection cache [usually in a folder named "emojidex" or a hidden folder named 
".emojidex"]. Each collection folder for each client will share the same layout and same types of 
commonly parsible files.

emoji are generally obtained from emojidex from the master collection, but there are two special 
static collections; the UTF and Extended collections.

emojidex Collection
-------------------
These are all the emoji available on the emojidex service, searchable and indexed in the emojidex 
API. This collection DOES contain the UTF and Extended emoji - so in general this is the only 
collection you ever need to worry about. The UTF and Extended collections generally exist only for 
seeding and for clients with Closed Licenses who are not using the emojidex service.

UTF Collection
--------------
The UTF collection contains a completely original set of all the emoji with UTF codes (the Unicode 
Standard emoji). The index can be found at http://assets.emojidex.com/utf/emoji.json and assets 
can be found at standard locations, with SVG found at 
http://assets.emojidex.com/utf/{:emoji_code}.svg 
and PNG found in sized folders such as 
http://assets.emojidex.com/utf/px32/{:emoji_code}.png .

<aside class="notice">
This collection can be found within the emojidex Collection by obtaining emoji from the "emoji" 
or "絵文字" users. See the API documentation [User Emoji](/#user-emoji) section for details.
</aside>

Extended Collection
-------------------
The Extended collection contains a completely original set of all the emoji we wanted to see. 
Extended emoji DO NOT have UTF/Unicode entries. The index can be found at 
http://assets.emojidex.com/extended/emoji.json and assets 
can be found at standard locations, with SVG found at 
http://assets.emojidex.com/extended/{:emoji_code}.svg 
and PNG found in sized folders such as 
http://assets.emojidex.com/extended/px32/{:emoji_code}.png .

<aside class="notice">
This collection can be found within the emojidex Collection by obtaining emoji from the "emojidex" 
or "絵文字デックス" users. See the API documentation [User Emoji](/#user-emoji) section for details.
</aside>

UTF/Extended Packages
---------------------
The UTF and Extended emoji are also available in static git repositories or as Ruby gems.
[emojidex-vectors](https://github.com/emojidex/emojidex-vectors) and 
[emojidex-rasters](https://github.com/emojidex/emojidex-rasters) repositories hold vectors and 
rasters respectedl. There is no need to use these unless you wish to bundle seed emoji with your 
application.

<aside class="warning">
Bundling emojidex assets statically with your application may violate the allowable terms in the 
emojidex Open License. To avoid this, either use up-to-date versions of standard clents or implement 
under the guidelines set fourth by the emojidex General License Terms and Conditions.
</aside>
