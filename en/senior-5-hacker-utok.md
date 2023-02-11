Hacker attack on the agency
===========================

> id: '7d5edd94-cce4-4a32-9ec5-51260dcd1653'
> slug:
> 	cs: senior-5-utok-hackeru-na-agenturu
> 	en: hacker-attack-on-the-agency
> 
> publicationDate: '2023-02-11 14:20:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: '18511f7b3ff80124720244a93e140932'

A story from 2017: you work as a lead developer in an agency, and you manage about 300 projects of various sizes that the company has developed in that time. Most of them are simple Nette applications with up to 10 templates, a few forms and database tables. Nothing fancy. You don't know that much about the projects, because each one was developed by a slightly different vendor, people rotate in and out of the company, and you're hoping to get it done somehow. Because the company does a lot of cost optimization, they host maybe 50 projects on one server at a time.

Suddenly, a project manager comes running to you saying that one of the important client sites has stopped working. Instead of the real site, you get a black screen, and there's all sorts of text saying that the site has been hacked, and has access to the entire server.

Once again, you don't (and can't validly) know that much about the architecture and content of projects, and many projects run on just the server. The projectionist is pushing you that the site must work, and yet you don't know the scope of the attack and wherever the attacker has gone, or if there are backdoors from the past.

How do you decide?

1. You do nothing and try to analyze the incident first.
2. You take the server offline. You don't know the extent of the damage and the attacker can continue to penetrate the server and steal data. At the same time, it means many sites are unavailable.
3. You perform a restore of a specific site from a backup a day ago, and in the meantime you address what happened. But the attacker can regain access and continue to steal data.
4. Another solution...
