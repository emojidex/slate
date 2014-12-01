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

## Asset Formats
All assets come in Frame Animated SVG and Animated PNG. Assets without animation will be 
regular SVG or PNG. To generate Frame Animated SVG we recommend 
[Phantom SVG](https://github.com/Genshin/phantom_svg). 
To generate Animated PNG we recommend 
[apngasm](https://github.com/apngasm/apngasm).

<aside class="warning">
In general PNG will be *SMALLER* in file size than SVG. Prefer using PNG over SVG 
unless you speifically need one image that will be displayed in many sizes.
</aside>

<aside class="warning">
Formats other than SVG or PNG will NOT be made available at this time.
</aside>

<aside class="notice">
There are no immediate plans to implent WebP (though if you submit a patch for 
[emojidex-converter](https://github.com/emojidex/emojidex-converter) we will 
certainly consider it.
</aside>

<aside class="notice">
NO, we will NOT offer animated GIF. Ever. Don't ask. Animated GIF is proven 
to be inferior in quality, size and features when compared to Animated PNG. 
If your platform does not support Animated PNG then submit a feature request 
FOR YOUR PLATFORM. There are already issues up for 
[Chrome](https://code.google.com/p/chromium/issues/detail?id=1171)
and 
[IE](https://wpdev.uservoice.com/forums/257854-internet-explorer-platform/suggestions/6513393-apng-animated-png-images-support-firefox-and-sa).
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
{ ldpi: 13, mdpi: 18, hdpi: 27, xhdpi: 36, xxhdpi: 54, xxxhdpi: 72,
  px8: 8, px16: 16, px32: 32, px64: 64, px128: 128, px256: 256, px512: 512,
  hanko: 90, seal: 320 }
```  
Each emoji size is squared (wide emoji coming soon!). An emoji in a particular size is always
guarnteed to have the height specified for its size. For example, LDPI emoji are always 13px 
high.  

display | size | address
------- | ---- | -------
![blush emoji](http://assets.emojidex.com/emoji/ldpi/blush.png)  | ldpi  | http://assets.emojidex.com/emoji/ldpi/blush.png
![blush emoji](http://assets.emojidex.com/emoji/mdpi/blush.png)  | mdpi  | http://assets.emojidex.com/emoji/mdpi/blush.png
![blush emoji](http://assets.emojidex.com/emoji/hdpi/blush.png)  | hdpi  | http://assets.emojidex.com/emoji/hdpi/blush.png
![blush emoji](http://assets.emojidex.com/emoji/xhdpi/blush.png) | xhdpi | http://assets.emojidex.com/emoji/xhdpi/blush.png
![blush emoji](http://assets.emojidex.com/emoji/xxhdpi/blush.png) | xxhdpi | http://assets.emojidex.com/emoji/xxhdpi/blush.png
![blush emoji](http://assets.emojidex.com/emoji/xxxhdpi/blush.png) | xxxhdpi | http://assets.emojidex.com/emoji/xxxhdpi/blush.png
![blush emoji](http://assets.emojidex.com/emoji/px8/blush.png)   | px8   | http://assets.emojidex.com/emoji/px8/blush.png
![blush emoji](http://assets.emojidex.com/emoji/px16/blush.png)  | px16  | http://assets.emojidex.com/emoji/px16/blush.png
![blush emoji](http://assets.emojidex.com/emoji/px32/blush.png)  | px32  | http://assets.emojidex.com/emoji/px32/blush.png
![blush emoji](http://assets.emojidex.com/emoji/px64/blush.png)  | px64  | http://assets.emojidex.com/emoji/px64/blush.png
![blush emoji](http://assets.emojidex.com/emoji/px128/blush.png) | px128 | http://assets.emojidex.com/emoji/px128/blush.png
![blush emoji](http://assets.emojidex.com/emoji/px256/blush.png) | px256 | http://assets.emojidex.com/emoji/px256/blush.png
[(image too big, click here to see)](http://assets.emojidex.com/emoji/px512/blush.png) | px512 | http://assets.emojidex.com/emoji/px512/blush.png
![blush emoji](http://assets.emojidex.com/emoji/hanko/blush.png) | hanko | http://assets.emojidex.com/emoji/hanko/blush.png
[(image too big, click here to see)](http://assets.emojidex.com/emoji/seal/blush.png) | seal | http://assets.emojidex.com/emoji/seal/blush.png

<aside class="warning">
Only obtian the images you need only in the size that matches your use case or the nearest size 
that is larger than your use case.
</aside>
<aside class="warning">
Do not agressively download/cache emoji you may not be displaying. If you are going to pre-cache 
some emoji images please limit it to only a few (eg. 10 or less).
</aside>

In general small text will usually be best with an emoji 
size of about ldpi ![blush emoji](http://assets.emojidex.com/emoji/ldpi/blush.png) 
and web text will be around mdpi ![blush emoji](http://assets.emojidex.com/emoji/mdpi/blush.png). 
Thicker text readable on handsets will usually be around hdpi 
![blush emoji](http://assets.emojidex.com/emoji/hdpi/blush.png). Android developers will note the 
naming conventions coincide with standard asset quality naming conventions; they are in fact 
sized to be compatible with Android standard sizing. iOS and other platforms should find this 
sizing convention entirely compatible with native sizing guidelines.  


### non-inline emoji / emoji as Media Assets
Looking at the sizing chart you will notice the "hanko" and "seal" sizes. Hanko「はんこ」(Japanese 
for "stamp") is a mid-sized asset perfect for "stamping" on images or adding into multimedia 
and non-text content. It may also be used as a smaller sized asset for messaging clients.  
  
The "seal" size is made specifically for messaging clients and social media feeds to be sent as 
individual images in timelines or as individual messages. It will perfectly occupy the width of 
most messaging clients or social media feeds without distortion or any degrade in quality.

