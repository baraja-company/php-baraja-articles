How a programmer lives in an internal development team
======================================================

> id: '9309b0f2-0265-43aa-a9c3-10b2d486cdfc'
> slug:
> 	cs: jak-zije-programator-v-internim-tymu
> 	en: how-a-programmer-lives-in-an-internal-development-team
> 
> publicationDate: '2022-01-10 12:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: e65dd78fa9813b552f30b98f04aacb30

There is a heavy price to pay for honesty.

This site has always been a description of the reality that people in IT experience, so I'd like to look at my experience working on development teams. The following are general experiences I have had across companies. No experience is associated with one particular company and does not necessarily serve as a criticism.

Companies tend not to want hard-working and proactive people
----------------------------------------------

Got lots of ideas? Do you want to innovate? Do you enjoy finding elegant solutions to the complex problems your team is solving that are plaguing half the company? Do you realize the importance of security, software design, and finding project bottlenecks?

You'll probably be unhappy on the development team for a long time.

Teamwork is something I've been struggling with a lot lately - I mean, I'm specifically swimming in it and trying to understand the completely unintuitive principles for me to adhere to.

- Teamwork means developing the solution that the whole team understands. It's often not the best. It's almost never elegant. In fact, it's always inefficient. But it has the cool advantage that the whole team understands it and it can be managed over the long term. This is an extremely important idea. Because with big projects, there's not that much pressure to be efficient because as a company you can scale through people and you have enough money. It's much more important to deliver things on time, even if partially broken and with unknown constraints in advance, but on time. It has to do with the idea that the solution you deliver tomorrow (even if imperfect) has much more value to the customer than a 100% debugged solution that won't be around for a year.
- Innovating and pushing things out is rather wrong. It has to do with the fact that there are real people working in companies who have families and only want to work during business hours. It necessarily follows that people have limited time to learn and try new things. Essentially, companies have to cram education and development into people's working hours that can be used for future work. An interesting consequence then is the postponement of decisions and learning new things to the necessary time. For my part, I understand this approach and it makes a lot of sense to me. If I had employees, I would also prefer calm people who will last in the company for, say, 15 years, over people who have a great general overview and want to move on, but it will be at the expense of the team. It's true that the collective solution will never be the same as the individual solution, but that doesn't mean it will be worse.

People like me are a potential source of problems and conflict. It's cool to think of it that way.

When you codereview, just look for objective errors
----------------------------------------

Every company has its codestyle rules. Unfortunately. It annoyed me so much at first.

When you use one of the more mature languages, like .NET, the codestyle rules tend to be more similar. It's only then that you find out that there's still PHP and JavaScript in the world. Especially the PHP thing is a bit frustrating at times. PHP is a very mature language with many great features, but in my experience, it is used in companies at about a third of its total potential. The reasons tend to vary, most often inertia.

In this regard, I have long been looking for a procedural solution to code review. I have been playing with PhpStan for a long time and following the ideas around it. The basic idea behind PhpStan is that it only highlights objective errors that are certain to occur at some point. The more I explore PhpStan and take it to the next level, the more I internally validate how true it is.

It would be nice if PhpStan would be implemented in at least half of Czech companies. It would be even better if at least to level 6. In practice, I have experienced that the best companies have level 4 or 5.

Just the other day I was wondering if there can be a real running production application that has its own development team, and it doesn't even pass level 0 - that's the level that controls the things you would expect in any application. Something along the lines of making sure all classes and functions exist, files don't have parse errors, and so on. And yes, there are such applications. But the situation is improving in the long run. Hopefully.

So I follow a similar approach with codereview. I only report things that are objectively always wrong. I often run into misjudgment of degree - something is wrong, but again, it's not such a big problem that it needs to be addressed. Many problems are solved by convention, or by declaring that the risk is small, so there's no point in treating it.

I've always considered missing data types (even compound ones) to be a critical bug. Then I understood that there is test and dump.

I don't think I have a concrete conclusion to this part. I just have a lot of subjective comments about it, which are more likely to be a source of mutual misunderstanding, so I won't post them. Long term, I'm inclined to the conclusion that I can't do codereview - either I'm too strict or too benevolent. I can't tell at a general level what really matters and what isn't so important. I'd be quite interested to know how other people do it. No one has yet been able to give me an answer that I can use, too.

There's no money in custom development.
---------------------------------

Do you have an agency and do custom web development? If you're not big enough and doing big projects for other corporations, you probably won't exist one day.

The reason this happens is because of poor funding for custom projects. It probably has to do with the nature of the contracts, which are more profitable for the buyers. In fact, most agencies are brutally underfunded and only make a real profit for the owners.

When you internally develop a project as a company for yourself, a delay in delivery by a month probably doesn't matter that much. As an agency, if you don't deliver a job by a month, you're guaranteed to be slow asleep at work that said month, consistently ruining your health, the client swearing into the phone and tickets, always asking if it could be faster, and as a reward you'll still pay a contractual penalty. And if you don't, the client will label you as an unreliable contractor and may consider going elsewhere, where it won't be any better anyway.

But those are the rules of the game, which I've come to know in practice, and which have put a hole in my budget.

Contracting is probably the most complex form of business in IT. You don't want to do contracting. Contracting will destroy you. They'll call you on the weekend, at night, at Christmas. Half the work you do, you don't get paid. You'll be apologizing for mistakes you can't make. You'll be looking on Instagram at the storytelling of friends who went on their third vacation this year while you sit in your office at 3am on a Saturday finishing up a client project that you'll get scolded for on Monday anyway because there was more in the contract than you could get done. People will take vacation and sick days and you'll work in their place. Or you get terminated. And then you'll be happy to pay the rent.

But then again, you learn a lot. Because just under pressure, you have a lot of pressure to learn and be efficient so that next time you can get a little bit more done in the same amount of time. The bad thing is that you can't really apply that knowledge and experience elsewhere because companies just aren't interested (I explained above).

Fortunately, this is a 2019 story and it's a long way off.

There will never be an ideal world
-------------------------

If I'm doing what I enjoy? Do you? :D

I guess it's all a matter of proportion. You will probably never do a job that is 100% fulfilling. You'll probably always have someone explaining to you that you can't do the thing that you've been forcefully learning for 10 years and you know internally that you can.

On the other hand, for a certain amount of denial of your own personality, you gain the space to have something more. Like weekend time, better health, more time for yourself, a stable environment, etc. That it can't be sustained in the long run is obvious too.

I like being able to try different things to experience first hand how others have it. Working on a development team is a huge bore compared to a custom job.

It's no longer about finding the best and fastest solution, it's about collaboration. It's about improving communication, emotions, and most importantly, learning to be human. And that's a huge value, too.
