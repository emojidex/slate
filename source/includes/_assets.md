#Assets

## Assets [CDN] Address
`http://assets.emojidex.com`

Note that this is not (currently) an HTTPS address. Assets are hosted in an S3 bucket. 
In the future we will try to rig SSL up to the assets subdomain but at the moment SSL 
can optionally be used by allowing certs from \*.s3.amazonaws.com. You could directly 
access assets by using the the Amazon S3 address but we DO NOT recommend it:

`https://s3-us-west-2.amazonaws.com/assets.emojidex.com/`

<aside class="warning">
The S3 address may change at some point. DO NOT use the S3 address in any static content. 
ONLY use the S3 address if you absolutely need HTTPS/SSL AND you can not override the 
certificate origin AND you are prepared to change references to that address at some point 
in the future.
</aside>

## Asset Access / Layout

> Get the 32px PNG image for :blush:

```
http://assets.emojidex.com/emoji/px32/blush.png
```

> Get the SVG image for :blush:

```
http://assets.emojidex.com/emoji/blush.svg
```

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

