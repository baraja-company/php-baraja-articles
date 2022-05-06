How to break the md5 function
=============================

> id: '9e678fcc-3d5e-46a3-9c3f-c1eb3d1ad367'
> slug:
> 	cs: lamani-md5
> 	en: how-to-break-the-md5-function
> 
> publicationDate: '2019-08-23 15:33:10'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '4e176a09e43dbf12e131103be6a9cf4f'

MD5 is a very commonly used function for calculating hashes.

Beginners often use it for <a href="/hashovani">password hashing</a>, which is not a good idea because there are many ways to retrieve the original password.

This article describes specific methods for doing so.

Time complexity
----------------

All security is built on the fact that it takes a disproportionately long time to try all passwords. Well, it should. The problem with the `md5()` algorithm in particular is that it is a very fast function. On a normal computer, it is no problem to compute over a million hashes per second.

If we break the password by trying combinations one by one, it is a **brute force attack**.

Cracking methods
----------------

There are several strategies:

- Sequential trial and error (brute force attack)
- Testing dictionary passwords
- Rainbow tables (precomputed hash database)
- Google searches
- Hitting collisions in the algorithm

There are many more methods, this article describes only the most common ones.

Brute force breaking strategies
-----------------------------

All combinations of letters, numbers and other characters are tried one at a time.

The generated attempts are hashed one by one and compared with the original hash.

So, for example:

```php
aaaaaaabaaabaaacaaadaaaaaaaaaf...
```

The problem with this attack lies in the `md5()` algorithm itself, if we were to try only the lower case letters of the English alphabet and numbers, it would take at most tens of minutes to try all the combinations on a commonly available computer.

It is therefore important to choose long passwords, preferably random ones with special characters.

Dictionary attack strategy
----------------------------

People usually choose weak passwords that exist in the dictionary.

If we take advantage of this fact, we can quickly discard unlikely variants such as `6w1SCq5cs` and instead guess existing words.

Furthermore, we know from previous password leaks of large companies that users choose a capital letter at the beginning of the password and a number at the end. Let's see - does your password have that too? :)

Rainbow tables - precomputed database
--------------------------------------

Since one password always corresponds to the same hash, you can easily recalculate a huge database in which passwords will be searched first.

In fact, searching is always orders of magnitude faster than searching hashes over and over again.

In addition, for larger data leaks, passwords can be hashed in parallel in this way and, for example, 10% of all user passwords can be quickly retrieved.

A good password database is for example <a href="https://crackstation.net/">Crack Station</a>.

Google search
-------------------

Many simple passwords are known directly to Google because it indexes pages that contain hashes.

I always use Google as my first option. :)

Finding collisions in the algorithm
--------------------------

The <a href="https://cs.wikipedia.org/wiki/Dirichlet%C5%AFv_princip">Dirichlet Principle</a> describes that if we have a set of hashes that are always 32 characters long, then there are at least 2 different passwords of 33 characters (one longer) that generate the same hash.

In practice, it doesn't make sense to look for collisions, but sometimes the application author himself makes the guessing easier by recomputing the collisions.

For example:

```php
$password = 'password';

for ($i = 0; $i <= 1000; $i++) {
    $password = md5($password);
}

echo $password; // hashed 1000 times via md5()
```

In this case, it makes sense to guess the collision rather than the original hash.

Cheers to basting!
