How to select technologies? When do we switch to JavaScript?
============================================================

> id: d3ea7ea7-3e2f-455d-a424-cce374bcd1d2
> slug:
> 	cs: senior-9-vyber-technologii
> 	en: how-to-select-technologies-when-do-we-switch-to-javascript
> 
> publicationDate: '2023-02-11 14:40:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: '72807451867478e1a7b6d67f4e5c31b3'

Choosing the right technologies is a prerequisite for becoming a senior developer. These decisions are often not easy, because you have to take into account the current technical state of the application, where you are going developmentally, what your current team knowledge is, what knowledge is common in the job market, what the costs of each technology are, what risks it will bring to your operation, how secure and stable the technology is, and last but not least, what will developers be interested in, say, 5 years from now when 80% of your current team is replaced.

I've been through 6 large companies that develop in PHP. Only 2 of them are trying to switch to another technology in the long run, the others are staying. That has a lot of problems. For example, I'm currently trying to find a senior PHP developer for a corporate project I'm developing for O2, with the requirement to commute to offices in Prague, and I can see how the PHP developer market has cleared in the last 5 years. PHP just isn't cool anymore and not many people want to do it. There are not enough juniors in it.

From interviewing younger people, I perceive that React and generally "thin" technologies are very popular nowadays. From an application architecture perspective, it makes sense if you discover this direction early on, and have time to adapt. Instead of the complex pounding of web layouts and forms in Latte, for which you need a practically mediocre developer for an already slightly more complex assignment, in React you just need a junior who started basically a month ago, and still doesn't make too many mistakes in the future solution.

React lets you throw away a big chunk of the backend that was written just so the frontend could exist in the first place. In short, it makes development cheaper, and as a bonus you get faster delivery of new features because developers don't have to deal over and over again with the complex problems that come from the PHP design language.

Most web applications don't even need a backend anymore, or only a minimal backend. When you expose API endpoints in Node.js (also a technology built on javascript), suddenly a developer who was previously only doing React can write pieces of the backend as well, because it's the same language.

In a deeper analysis of the projects I've developed over the last 5 years, there are only a few things missing in Node.js that still make me use PHP for some operations.

Namely:

- Doctrine (and in general relational database access based on object entities)
- Sending and managing emails
- SOAP API (unfortunately it's still around sometimes)
- Sessions (you have to replace it with a JWT token, for example)
- Complex legacy logic that was written in PHP and I can't easily refactor it
- Fast processing of complex data structures where data needs to be mutated
- Existing people on the team that you have to retrain to do something new

But then along comes Node.js, which does the rest of the stuff better. For example:

- The ability to offload the app straight to the cloud
- Much (maybe even twice) cheaper development of the same functionality
- Same logic on BE and FE without having to write code twice
- REST API endpoints
- Parallel calls to multiple code at once
- Ability to send an HTTP response, but the code continues to run
- Crony
- Libraries to work with cloud services
- Significantly better response time because you don't have to boot a huge application
- Fully functional paradigm (you get rid of DIs you don't need in JS, for example)
- Work with forms and data
- Easy updates and an active developer community

**Comment on Doctrine:** I know JS brings a lot of libraries for working with databases. Or even new paradigms like Mongo. I like where the data processing direction is heading. On the other hand, I believe that tabular relational databases will never go away. When you're doing a really big project where you're managing upwards of tens of millions of records, you simply need traditional technology that you're very familiar with and know what to expect. For example, the idea that I want to add a column (property) and it would mean remapping all the entities with a migration script is pretty scary to me.
