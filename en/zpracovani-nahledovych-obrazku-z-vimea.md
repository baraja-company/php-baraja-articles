Processing thumbnail images and meta information from Vimeo
===========================================================

> id: '8b8cbef0-210a-4883-be07-5004e8f43739'
> slug:
> 	cs: zpracovani-nahledovych-obrazku-z-vimea
> 	en: processing-thumbnail-images-and-meta-information-from-vimeo
> 
> publicationDate: '2020-09-19 17:32:31'
> mainCategoryId: a0143f3c-ac75-46dc-a514-d3c9417ded4e
> sourceContentHash: '93c29218f162fb5dc8d0f265968b0f8d'

When embedding videos from Vimeo into a page (as an HTML embed), we may often want to also get the image and other useful information such as video length, full title, author, and so on.

Fortunately, Vimeo provides a simple HTTP API from which we can read all the data based on the video token.

To avoid having to write the API yourself, just use [ready package](https://github.com/baraja-core/vimeo-video-api), which integrates the API completely.

You install the package with the command:

``shell
composer require baraja-core/vimeo-video-api
```

It is easy to use. You create an instance of the `\Baraja\VimeoAPI\VimeoVideoAPI` service to communicate with Vimeo according to the documentation, call the `getInfo()` method, pass the video token, and get the details as a `VideoInfo` entity from which all available information can be read (not all information is always available for every video).

This way you can query even private and publicly unavailable videos. But you always need to know their token.

Listing all available information
---------

A basic way to use the library looks like this:

```php
$api = new \Baraja\VimeoAPI\VimeoVideoAPI;

$token = 0; // Video token as integer
$info = $api->getInfo($token);

echo var_dump($info); // dumps everything

// Dumps the length of the video in seconds:
echo 'Video length is: ' . $info->getDuration();
```

The `$info` variable stores all the descriptive information about a particular video. For an overview of all available methods [see the implementation](https://github.com/baraja-core/vimeo-video-api/blob/master/src/VideoInfo.php).
