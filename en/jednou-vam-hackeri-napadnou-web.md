One day hackers will attack your website
========================================

> id: a11ca0a6-77e1-4ba4-8eca-83449c9cbf48
> slug:
> 	cs: jednou-vam-hackeri-napadnou-web
> 	en: one-day-hackers-will-attack-your-website
> 
> publicationDate: '2020-08-04 12:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '0f0ac4c091115e9c1f0833d318f34f5d'

That's not gonna happen to you? :DDDDDDD

While managing more than 300 websites, I get various emergencies from time to time. Sometimes they are quite heated, but often it is a complete triviality. If, like me, you have been tempted by programming in the past, and you also know that [programming is a pain](http://borisovo.cz/programming-sucks-cz.html), you will surely agree with me.

In the grip of evil hackers
----------------------

When a web application becomes popular, it becomes a tempting target for hackers. Their motivation is usually not to bring down the entire website, quite the opposite. In fact, **hackers want you, as the server administrator, to be completely unaware of them**. A good hacker waits for months, watching the web server and getting the most valuable thing - your data.

To protect your users, it's imperative to encrypt all data and have multiple layers of protection. For passwords, use one of the slow functions such as [Bcrypt + salt](https://php.baraja.cz/hashovani), Argon2, PBKDF2, or Scrypt. Michal Špaček has already written about [security rating](https://pulse.michalspacek.cz/passwords/storages/rating#slow-hashes), and I thank him for adding to the article. As a site owner, you must insist on proper password hashing when discussing with a programmer. Otherwise, you are going to cry, just like many people before you who also deluded themselves that it didn't concern them.

Can you tell when you're under attack?
---------------------------------

I've noticed that when a web server reaches a certain threshold of traffic (in the Czech Republic, it's about 50+ visitors per day), it starts to get a series of attacks directed at it out of nowhere. To make it not easy, the attacker always has an advantage, because he can choose which part of the infrastructure to start attacking and you - as the owner of the web server - have to recognize it in time, defend yourself and be better prepared for it next time.

How many attacks have been directed at you in the last month, for example? Do you know exactly? How many of them have you defended? Does anyone else have access to your server?

What if someone is attacking you right now? And now... and now also...?

Unfortunately, there's no one-size-fits-all way to recognize an attack. But there are tools that can give you a significant advantage.

What's worked best for me lately:

- **When an attacker doesn't know where your servers are physically**, they have a much more difficult position. Physical architecture information can be obfuscated very well with [Cloudflare](https://www.cloudflare.com/), which proxies (and bi-directionally encrypts) all network communication. It also has a number of other advantages, such as faster retrieval from abroad, automatic protection against DDOS attacks, asset compression, and last but not least, automatic blocking of rogue visitors. I use Cloudflare for all my websites (and almost every project uses different technologies).
- **Error logging**. Always and everything. Chances are your application contains a lot of bugs committed by your programmer. Be it [uncaught exceptions](https://php.baraja.cz/vyjimky), application errors, SQL injection, and so on. If you're programming in PHP and don't understand logging, there are enough problems that [Tracy](https://tracy.nette.org/) (and possibly other tools like [Sentry](https://sentry.io/)) and access logs on the server itself will catch. Error logging is not a nice to have, it's a must. I log bugs on all the sites I actively maintain, and have information about new bugs sent to my email so I know about it immediately.
- **Secure and updated platform**. Do you have WordPress on your site? Do you know how to update it properly? Do you know all the risks you face? When something is free, it's almost always at the expense of something else. Personally, I use [Nette framework](https://nette.org/cs/) version 3 for application development, which I update weekly and actively look for security vulnerabilities to patch. Just like you have your car serviced regularly, you need to have your website serviced regularly and at least once a month. Site maintenance includes updating all external libraries and consistently monitoring logs. If you are using an old version of a library, it is very likely that an attacker knows about the vulnerability and will try to exploit it.
The question is when
A large number of people in my area underestimate security in an incredible way and expose content to the Internet that is very easy to break and exploit.

In fact, even large companies make mistakes, even on trivial things like the [XSS](https://www.zive.cz/clanky/vyvojar-objevil-zranitelnost-v-seznamu-dokazal-mezi-vysledky-propasovat-zakazany-kod/sc-3-a-200023/default.aspx) (cross-site scripting) attack.

The question is not whether someone will ever break your website. The question is when it will happen and whether you will be prepared enough to deal with it. Maybe a hacker has had access to your server for months and you don't know it (yet).

What to do when trouble happens
-------------------------------

By the time you find out about the hacker's work, it's usually too late and the hacker has done everything they needed to do. He has very likely placed malware all over the server and has at least partial control over the machine.

This is a **chaotic type of problem and you need to act fast**.

Since this is the last stage of the attack, I personally recommend disconnecting the web server completely from the internet and start to gradually figure out what all happened. Plus, we will never know exactly if the server is hiding something else. The attacker always has a significant head start.

If we decide to put the last working backup back on the server, we will be helping the attacker a lot. He already knows how to break into your server and there is nothing stopping him from doing it again in a few hours. In addition, the backup probably contains malware or some other type of virus.

Although this is a very unpopular option among my clients, I personally always recommend **completely deleting WordPress sites** first, or any systems that the client doesn't update that have a lot of security threats. For example, if you host 30 sites on one server that are more than 2 years old, you are virtually guaranteed that at least one of them contains some form of vulnerability.

If you don't understand the issue, you'd better contact a suitable specialist early on who has deep knowledge of your issue and understands things in a broad context.

**Ethical Note:**

If you are managing a website for a client who has had a security incident, it is your responsibility to inform them. If any data (such as passwords, but often much more) is leaked, you must also inform end users that their password is public knowledge and they should change it everywhere. If you use a slow hashing function (for example, **Bcrypt + salt**), you will make it significantly more difficult for an attacker to crack passwords. (Unfortunately, [Bcrypt passwords can be cracked](https://arstechnica.com/information-technology/2015/08/cracking-all-hacked-ashley-madison-passwords-could-take-a-lifetime/)). If you wish to receive regular information about whether your account has been leaked, I recommend registering with [Have i been pwned?](https://haveibeenpwned.com/). For more information about [security breach](https://m.uoou.cz/vismo/zobraz_dok.asp?id_ktg=5020&n=poruseni-zabezpeceni), please visit the OOOO website.

Atomic Habits
--------------

In the last six months I finished reading the book [Atomic Habits](https://www.melvil.cz/kniha-atomove-navyky/), which was an eye-opener. It describes a process of incremental 1% improvement every day that will do a lot in the long run.

Since I am approached by companies and individuals at various stages of technology debt, I would like to summarize the worst things:

- **FTP for server communication doesn't make sense**. It is an insecure protocol that is password controlled. If you want to give someone access, they need to know the password and can do the same thing you do. If you want to take away his access, you can't, the only way is to change the password. Better to completely switch to SSH and use RSA keys. Quite often I'm surprised that even companies solve this problem these days. Never make one table of all passwords. And certainly never a shared one for the whole team!
- The **Source code belongs in Git**, the data in the database and the static content (images, PDFs, ...) in files on disk. Choosing the wrong data storage makes the application design and security worse. The URL to a PDF (for example with an invoice) should contain randomly generated content that only the recipient knows. For extremely sensitive content (such as a bank statement), it is important to encrypt the content on disk and only provide it after login.
- Non-public parts of the site must contain the META header `noindex` and `nofollow` to avoid being indexed by Google. Sensitive content that your competitors must not know must be password protected.
- Disable [directory content listing](https://www.simplified.guide/apache/disable-directory-listing) on your server. If set inappropriately, the entire site can be machine crawled to find sensitive files.
- Project root! There must never be a direct URL to configuration files. For example, [Nette does it right](https://doc.nette.org/cs/3.0/quickstart/getting-started#toc-obsah-web-projectu) right from the first tutorial. The same goes for [publicly available git repositories](https://smitka.me/open-git/). This type of vulnerability is extremely easy to underestimate and the consequences tend to be tragic.
- The bug can also arise from an inadvertent development mistake. It is very important to do a codereview by a live human for each change. Many bugs are discovered by [PHPStan](https://github.com/phpstan/phpstan) or similar tools before a human sees the code.
- Never [send passwords in readable form by email](https://www.lupa.cz/clanky/reset-a-poslani-hesla-v-citelne-podobe-e-mailem-nebezpecna-praktika/), there's always another way to solve it.
- Security problems in old versions of libraries and software. Robots crawl the internet every second and attack known security holes (SQL injection, XSS, CSRF, ...). A lot of known holes have older versions of WordPress.

There are many more vulnerabilities and the problems vary from project to project. If you need to do a quick server audit, I recommend using [Maldet](https://www.rfxn.com/projects/linux-malware-detect/) and then hiring a suitable specialist to help you do a [full site audit](https://baraja.cz/audit-webu), and not just for security reasons.

Security engineers are like plastic in the ocean - just forever. Get used to it.

The action and reaction principle of nature
-------------------------------

You will also have noticed that nature almost always uses the reactionary principle. This means that something happens in a particular environment and the surrounding organisms react to it in different ways. This approach has many advantages, but one huge disadvantage - you will always be left behind.

As a thinking person (designer, developer, consultant) you have a great advantage, and that is to be actionable - that is, to know in advance a certain portion of the big risks and actively prevent them from happening in the first place. You may never prevent all screw-ups, but you can at least mitigate the consequences, or build control mechanisms that detect problems early so you have time to react.

Most attacks take place over a long period of time, and if you log, you will usually have enough time to detect the problem.
