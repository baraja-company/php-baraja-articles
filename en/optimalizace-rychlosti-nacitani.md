How to effectively optimize page load speed
===========================================

> id: '72f1d564-cb70-431c-a3dd-a3fdcaf14292'
> slug:
> 	- null
> 	en: how-to-effectively-optimize-page-load-speed
> 
> cs: optimalizace-rychlosti-nacitani
> publicationDate: '2021-10-15 15:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '013c8f29c213d3f3f1df3fbe4be8d572'

When I travel, I often encounter poor internet connections, which as a web designer makes me think about design principles to solve web speed even on a poor connection.

I've come up with a few useful tricks from practice:

Important landing pages are often simple and it pays to use custom CSS styling
-----------------------------------------------------------------------------------

For example, the login page to the CMS is often very simple. Does a simple form really need to download the whole Bootstrap and other CSS/JS frameworks? Important pages are worth optimizing and writing native code.

Once the page is fully rendered, we get a few seconds where the user fills in their details, and we can download and cache the remaining styles in the browser in the background. By the time the user logs in, they'll probably already have Bootstrap downloaded (and if they're on Edge, they won't...).

Instead of icons, we can often use emoji
-----------------------------------

Really! Emoji has a lot of practical advantages:

- It only takes up 4 bytes, so it trumps any image
- It never fails to download, so the user always sees at least an icon rather than a blank space
- Displays in the user's visual style
- Currently already has broad device support
- Saves the server request and we don't have to deal with where to get the image from (this can be optimized via CDN, but why, right)
- A lot of icons you probably don't want to deal with. For example, where to get the flag icons of all the countries you want to display in the administration? Use emoji: https://github.com/bara.../country/blob/main/flag-emoji.json

Use critical styles directly in the HTML when loading the site
------------------------------------------------------

I sometimes put the important CSS styles defining the page layout and basic layout directly into the HTML. I put a limit of 1 KB, which I try to get as much into as possible. The downside of this approach is that styles have to be transferred in each request and cannot be cached, on the other hand they download incomparably faster than an image.

I'm not such an expert on loading speed, [Martin Michalek](https://www.programia.cz/rozhovor-martin-michalek-rychlost-webu/) would tell you better.

Use ajax!
--------------

I always use ajax to retrieve unimportant or slow content. It adds a little bit to the technological requirements of the application, but I can afford it.

An example of a good place to use ajax is almost everything in the administration. Very often there is a lot of data to be retrieved, but the user is not interested in everything. When the user only has a thin javascript client downloaded and all the data is fetched via Vue and json, only the minimum amount of data is ever downloaded and the responses are instantaneous.

How to do this in Vue: https://vue.baraja.cz/api-a-ajax-ve-vue-js

On the backend, I use the library for Nette: https://github.com/baraja-core/structured-api

Use a suitable CDN
---------------------

For static content distribution, I recommend using a CDN for all types of sites. If you don't know how to set up a CDN, at least use Cloudflare in proxy mode. It will cache static content automatically to itself based on HTTP headers. Not every hosting provider has Pops set up well, and for long routes this will save you hundreds of milliseconds in each request.

Suitable image format
---------------------

One of my juniors recently put a PNG image on the main page of the site, which contained a photo and took up 3 MB. He said it was OK because the page loaded quickly on his connection (it does on his optics at home, doesn't it...), plus he said we transfer a lot of data these days, like video.

I'm old fashioned about this and optimize what I can.

Photos to JPG, or better WEBP. But saving a picture to JPG doesn't mean anything, you need to run it through an optimization service: https://tinyjpg.com

If you have images in Git, this tool can automatically compress them and send a pull request: https://imgbot.net. When a new image is added, it sends the PR again.

If you need to compress thousands of images (like a whole gallery of products on a site), I use a desktop application for Mac to do it: https://imageoptim.com/mac

Compressing images appropriately will usually save the most data. Sometimes even 50%.
