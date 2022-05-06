Caesar's cipher - how it works
==============================

> id: '662dd627-c106-406e-ad6a-7ae9a492ad92'
> slug:
> 	cs: cesarova-sifra
> 	en: caesar-s-cipher---how-it-works
> 
> publicationDate: '2019-08-23 15:43:02'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '892ad0746be469b5cdbb4de72d8e85b9'

> **Warning:** This article was written many years ago and some information may be outdated or incorrect. Please bear this in mind when reading.

The Caesar cipher is one of the simplest hashing functions. In its day it was virtually unbreakable, but in the age of modern computers it only takes a few tens of seconds, even a few minutes, to break it. It is based on a key, according to which the message is encrypted and then according to which it can be expanded again. The key is therefore secret. At the time of encryption, the message can be seen and will mean nothing (just a jumble of characters). The only way to break the cipher is to guess the key.

The key
--------------------------

The key can be any integer that has fewer digits than the message itself. Usually 3 valid digits are given (so there are 999 combinations). Each additional digit increases security. In order for 2 parties to communicate, both parties need to know this secret key (so they must somehow pass it to each other securely). If the key is known to them and not to the other, then the message can be spread even insecurely and potentially not compromise the content, because the potential attacker does not know the procedure to get the message back.

For demonstration purposes, I will use the key **123**, normally some other random number is used which is assumed not to be so easy to guess.

Encryption procedure
--------------------------

The principle is based on the idea of replacing the characters of a message with some other using a key. I call it "character shifting".

For example, let's have this message that we want to encrypt:

```php
SECRET MESSAGE
```


Now assign a number to each character. Typically alphabetically (its serial number). I will use the English alphabet, so this series of characters:

```php
A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
```


If we were to assign a number to each character, we would get something like:

```php
20-1-10-14-1 26-16-18-1-22-1
```


Now comes the key. We take each individual number and add the key to it. I'm highlighting in color what I added where:

`1 2 3`

```php
21 3 13 15 3 3 17 20 4 23 3
```


Note that when I encrypt the character **Z**, I write the digit 3. This is because **Z** is the last character of the alphabet, so when it reaches the end it is counted again from the beginning of the series.

Transmitting the message
--------------------------

Now we can pass the message any way we want. Sometimes even publicly. Others will only see an illogical series of numbers, so they probably won't even know it's some kind of cipher. Security is also enhanced by the key itself, which is secret. Sometimes it is appropriate to transmit the message as a series of numbers, sometimes it is appropriate to convert those numbers into characters (again, following the same alphabetical series) and then transmit a sequence of characters. Much depends on the circumstances. In general, however, a numeric sequence is better, because few people will suspect that it is an encoded message.

Decryption at the recipient
--------------------------

The recipient decrypts using the same procedure. It takes each individual character and subtracts the numbers according to the key and then converts the resulting values back to characters using the alphabet. It's just a reverse encryption procedure. The important thing is to know the **key** and the **character set** - that is, how the characters go in sequence.

Cracking and decryption
--------------------------

The only possible procedure for cracking is to try every conceivable combination of all potential keys. If we don't know the length of the key, the whole process gets even more complicated. But in general, today's computers can try something like 100 keys in 1 second, so a 3 character long random key takes something like 1 minute to crack.

However, if the key is as long or longer than the original message, it cannot be broken because each individual character has its own key, so all combinations must be tried for each character.

If I had a message "**TAKE MESSAGE**", it would be 11 characters long (spaces don't count). If I wanted a key that was also 11 characters long, and I used a 26-character English alphabet string, then there are **11^26** = 1.191817654*10²⁷ combinations, the average computer would crack that key in 1.310999419×10²⁶ seconds = 10^20 days :)
