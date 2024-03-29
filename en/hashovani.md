Hashing strings and passwords
=============================

> id: '7978bee8-62cc-4770-b15b-a8d08d1dcf34'
> slug:
> 	cs: hashovani
> 	en: hashing-strings-and-passwords
> 
> perex:
> 	- 'Hash není šifra! Metody hashování dat a hesel. MD5, SHA1, Bcrypt. Ověření hesla.'
> 	- 'Hash is not a cipher! Methods of hashing data and passwords. MD5, SHA1, Bcrypt. Password verification.'
> 
> publicationDate: '2019-09-11 10:13:30'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: f6ea0b06d6ace3c41684a49938f7ce8e

The hashing process (as opposed to encryption) produces an output from the input from which the original string can no longer be derived.

It is therefore well suited for protecting sensitive strings, passwords and checksums.

Another nice feature of hashing functions is that they always generate outputs of the same length, and a small change in the input always completely changes the entire output.

Hashing functions
----------------

There are many hash functions in PHP, the important ones are:

- **Bcrypt: password_hash()** - Most secure password hashing, computationally slow, uses internal salt and hashes iteratively.
- **md5()** - Very fast function suitable for file hashing. Output is always 32 characters.
- **sha1()** - Fast hash function for file hashing, used internally by Git for commit hashing. The output is always 40 characters.

Hashing
-----------

```php
$password = 'secret-password';

echo password_hash($password); // Bcrypt
echo md5($password);
echo sha1($password);
```

> **Warning:** Neither `md5()` nor `sha1()` is suitable for password hashing, because it is computationally easy to discover the original password, or at least to precompute the passwords. It is much better to use `bcrypt`, which was developed for password hashing.
>
> The <a href="https://www.md5cracker.com/">md5cracker.com</a> website has a database of checksums (hashes), try searching for hash: `79c2b46ce2594ecbcb5b73e928345492`, as you can see, so pure `md5()` is not that secure for common words and passwords.

The only correct solution: `Bcrypt + salt`
--------------------------------------

In the talk <a href="https://www.youtube.com/watch?v=F58_A5TM-Sc">How not to mess up in the target plane</a>, David Grudl addressed ways to hash and store passwords correctly.

The only correct solution is: `Bcrypt + salt`.

Specifically:

```php
$password = 'hash';

// Generates a secure hash
echo password_hash($password, PASSWORD_BCRYPT);

// Alternatively with higher complexity (default is 10):
echo password_hash($password, PASSWORD_BCRYPT, ['cost' => 12]);
```

The advantage of Bcrip is mainly in its speed and automatic salting.

The fact that it takes **long** to generate, say 100 ms, makes it very expensive for an attacker to test many passwords.

In addition, the output hash is automatically treated with **random salt**, which means that when the same password is hashed repeatedly, the output is always a different hash. Therefore, an attacker will not be able to use a precomputed hash table.

Therefore, we will not be able to verify the correctness of the password by repeated hashing, but will need to call a specialized function:

```php
if (password_verify($password, $hash)) {
    // Password is correct
} else {
    // Password is incorrect
}
```

Password salting
------------

To make hash cracking more difficult, it is a good idea to insert some additional string into the original input. Ideally a random one. This process is called **password salting**.

The security is based on the idea that an attacker will not be able to use a precomputed table of passwords and hashes, because he will not know the salt and will have to crack the passwords individually.

For example:

```php
$password = 'secret_passport';
$salt = 'fghjgtzjhg';

$hash = md5($password . $salt);

echo $password; // prints the original password
echo $hash;     // prints password hash including salt
```

Compound hash functions
------------------------

You might be thinking that it would be a good idea to perform the hash function repeatedly, thereby raising the complexity of cracking it, since the original password will need to be hashed repeatedly.

For example:

```php
$password = 'password';

for ($i = 0; $i <= 1000; $i++) {
    $password = md5($password);
}

echo $password; // 1000x hashed via md5()
```

Paradoxically, the difficulty of breaking through is reduced or remains almost the same.

The reason is that the `md5()` function is extremely fast and can compute over a million hashes per second on a regular computer, so trying passwords one by one does not slow down much.

The second reason is more of a theory, namely the possibility of running into a so-called collision. If we hash a password repeatedly, over time it may happen that we hit a hash that the attacker already knows, and this will allow him to hash the password using the database.

Therefore, it is better to use a slow secure hashing function and perform the hashing only once, while still treating the final output with salting.

Extremely secure comparison of two hashes/strings
---------------------------------------------------

Did you know that the === operator is not the most secure choice for hash comparison in password verification?

When comparing strings, it goes through the two strings character by character until it reaches the end (success, they are the same) or there is no difference (the strings are different).

And this can be exploited in an attack. If you measure the time accurately enough, you can estimate how many more characters are left to be added to get an exact match and reach the end, or you can estimate how far the strings have come when comparing strings.

The solution is to use the hash_equals() function wherever strings are compared, and it would matter if an attacker could find out the position where the comparison failed.

And how does the function do this? It makes sure that the comparison of any 2 strings always takes the same amount of time, so you can't tell by measuring the time where the difference occurred. I find some types of attacks really very unlikely and hard to implement. This is one of them.
