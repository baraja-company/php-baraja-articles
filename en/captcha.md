How Captcha works (descriptive image)
=====================================

> id: '9cf191e4-a49b-407b-b16e-21de7224ac43'
> slug:
> 	cs: captcha
> 	en: how-captcha-works-descriptive-image
> 
> perex: 'Captcha je druh obrázku, který ověří, že uživatel není robot a chrání webovou aplikaci. Možnosti implementace v PHP.'
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: '80357b2e7f0047bb488922f2ab7b4336'

Captcha is currently one of the most common ways to protect free formats. It was originally created not to protect data security, but to protect against spam and to recognize that it is a human.

However, it is machine-generated so it is not always perfect, but the success rate of a captcha test is somewhere around 99% and only 1% of images can be deciphered by a well-made spam robot.

How it works
--------------------------

There are more options, for example I use this solution:

- The server receives the information that the user has requested a form page and starts generating it.
- In the future captcha test field, it puts an image with a secret address that doesn't say anything (like a random number, a string of characters, ... whatever).
- The image is generated and stored at that address. At the same time, the correct transcript is written somewhere (perhaps in a session or database).
- When the user submits the form, it is checked if the transcript matches what he/she entered. If it does, the form is processed. If not, a re-description of another image is requested.
- After both successful and unsuccessful attempts, the temporary captcha image is deleted (never to be used again). If the form is not submitted within a certain time, it will be deleted as well (expires).

Correct transcription
--------------------------

If the captcha test can be solved, the user is probably human. However, it's good to think about users who can't solve the task, especially blind users. It's a good solution to combine multiple possible tests (especially voice prefetching). However, voice recognition by a machine is currently significantly more efficient than reading from an image. Therefore, this solution is not always ideal.

Custom simple captcha image
--------------------------

Often enough, it is enough to generate a blank image of certain dimensions and enter a few characters into it legibly without further editing. Seriously! Most spam bots are stupid and can't attack generic forms with this type of protection, even though it's perfectly readable text that better OCR can transcribe perfectly.

The resulting image may look like this:

```
<img src="captcha.php" alt="sample captcha">
```

Try refreshing the page a few times and you'll see that the code changes randomly each time. For demonstration purposes, it is just generated without saving, so it will be removed immediately after you load it.

I solved the source code using the PHPGD library, which is available on virtually every PHP installation and hosting:

```php
Header("Content-type: image/png");
$obr = ImageCreate(100, 35);
$background = ImageColorAllocate($obr, 219, 28, 49); //define background color
$bila = ImageColorAllocate($obr, 255, 255, 255); //define the white color for the text
$style = array($background);
ImageSetStyle($obr, $style);
  $random_cislo = rand(11111,99999); //draw a random number 5 characters long
  imagestring($obr, 5, 25, 10, $random_cislo, $bila); //function to render text (in this case a number)
ImagePNG($obr); //generate an image in memory and render it
ImageDestroy($obr); //delete the image from memory (no longer needed, as it is generated once)
```

Rendering the image is then just a matter of HTML:

```html
<img src="captcha.php">
```


Please note that this script is not functional on its own without further modification. It serves only as a demonstration to generate a simple image.

Summary
--------------------------

Captcha tests are fairly reliable, but annoying and time consuming. Sometimes they're not readable, so it's nice to give the user the option to load a different image using javascript so that the whole page doesn't have to be reloaded and everything has to be filled in again.

Remember: What is a trivial task for a human may never be an achievable solution for a machine. That's why even this primitive captcha with perfectly readable text can protect against more than half of spam.
