Asynchronous world view
=======================

> id: '7ed25269-659f-4d17-a642-d07480cbfafa'
> slug:
> 	- null
> 	en: asynchronous-world-view
> 
> cs: asynchronni-pohled-na-svet
> publicationDate: '2022-10-15 11:30:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '24d4ec106e7febec5ee84cdf86bd8f28'

I have noticed for quite a long time that our world has an asynchronous and decentralized nature. When you realize this, and start thinking about how to use it to your advantage, a whole grand concept of how to look at solving complex problems can easily emerge. In this post, I'd like to explain some of the ideas I'm already using. I don't get them from any particular source, it's more a combination of multiple experiences and my own ideas. These principles don't work for all cases.

Defining the environment and goals
-------------------------

Almost all projects I've ever tackled (either alone or as part of a team) have had a fairly specifically defined goal. This approach makes a lot of sense because it's important to know what we want. On the other hand, I believe that defining a specific goal is impossible in a complex environment, and often during implementation we find that we actually want to get somewhere else.

So instead of a specific goal, I build up an area of different goals where there are alternatives, or even a completely new isolated independent goal can be the right goal if it makes sense.

Examples from practice:

- I need to gradually expand into other markets. One of the sub-goals I discovered during implementation may be machine translation of the web.
- I need to pick up 6 shipments at different locations around Prague and I don't know the shortest route. I don't want to find it out in a complicated way, so I just head towards the furthest point and change my plan towards other routes along the way. For example, when I change at Anděl, I decide to take the tram that arrives first, which dynamically affects the next route plan.
- I need to write this post, but I don't have an hour of time in a row. Therefore, I can write it in parts while riding the metro as individual chapters. I will then do the final merge into this form in the future.
- I don't know how the algorithm works for calculating the merchandise redemption for my client, which I need to re-program. So I'll pose the query to multiple people at the same time and solve something else while I do it. Asynchronously over 2 days, multiple different answers will come in, which I will only make a decision based on later.
- I need to get rid of PHP on the server for a long time, so I'm gradually rewriting one page at a time into React. I'm gradually moving the sites to the cloud and building them on statically generated Nect.

None of the destinations are final destinations. If you're working with a surface and not a point, you're much more likely to hit it. At the same time, motivation works well here, because the worst thing that can happen is to reach your goal and then not have something to look forward to in the future.

A lot of it is also important to say that to have a similar mindset you need to have a fairly stable environment where you can make mistakes. This approach can't guarantee a specific deadline to solve a specific single task. On the other hand, it can optimize solving as many parallel tasks as possible in as little time as possible, though not necessarily in the order they arrived in the queue.

The principle could be compared to how computations work in parallel programming, for example. In short, you decompose the tasks into individual threads that solve different sub-tasks at once, and once you get all the answers, you can compose the solution.

Deploying new software versions
--------------------------------

I struggle with this a lot everywhere.

The way software development is usually handled is that you have some stable version that runs production for users, then you have some test environment where new development is done, and once in a while you move news out of the test environment for production, which is called a release. This process is relatively safe and slow.

Since I've gradually moved to a micro-frontend architecture, where the frontend of the application (what the user sees) is completely separated from the backend (where the computation and data processing happens), the release process can work differently.

I still have one production environment that always works. From it, a new branch is created for each task to address a specific feature. Because I use Vercel (a very good cloud provider) to host the frontend, I can have a custom URL for each development branch.

Once a new feature is tested, it is immediately merged and deployed to production in an asynchronous process. Therefore, it can commonly happen that there are maybe 15 new version deployments every day.

A lot of this relates to an idea that was conveyed to me at LMC - "A feature that you provide to a customer tomorrow, even if in an imperfect state, is of much higher value to them than a feature that is perfectly debugged but won't be available until a year from now". But this can only work if you have a way to distribute the news quickly - typically web development.

What I like a lot about the Next.js framework (built on Recto) and Vercel is the principle that you connect your GitHub repository directly to Vercel, and with each commit there's an automatic build, tests, and then deployment to production when everything is ok. So the developer doesn't have to worry about anything, and the app deploys every hour with zero effort. It is very important to formalize routine things and then automate them.

Content publishing
----------------

I have broken down higher dozens of topics and posts with me in Apple Notes. For each topic, I'm continually coming up with new ideas to jot down and sort them by tags. When multiple notes are generated for a topic, I convert them into chapters, and then convert the group of chapters into articles and FB posts.

Features of this principle:

- I don't know in advance how many articles I will ever publish,
- But then again, I know there may be a lot,
- I am able to ensure that each post is full of information, insights and ideas that have matured over time (this post took approximately 3 months to mature),
- It's sustainable and I'll enjoy it because I don't have to write a ton of text at one point and risk forgetting something.

And then the publishing process.

I publish a lot of content quietly on the web first, so that Google notices it first and the page gets indexed. When it has good keyword coverage and I'm happy with it overall, the topic goes on social media, for example.

For a lot of topics, I know they won't be of interest and more likely to annoy you. But at the same time, I want to have them online because someone may search for them in the future. In that case, the article stays only on the web. An example of these articles are the various overlay articles, which link the entire topic area so that I have as many topics covered as possible on the site. Cover articles are often more technical and boring. Or they are machine-generated categories where I just organize multiple articles into topics, thus covering as many keywords as possible, which someone may then want to search for.

Knowledge, education, testing
------------------------------

I like to play with technologies where it's not clear from the start what it will be good for one day.

Like machine translation. At first glance, it doesn't make sense to translate dozens of articles into foreign languages for readers I'm not in contact with. On the other hand, it may be one of the steps to make a decision in the future for which the prerequisites need to be met.

In general, no one knows what the future will look like. So it makes enormous sense to me to cover as many possibilities as possible that you want to understand, at least superficially, and perhaps address in the future. It's good to think of steps not just as a solved task, but as a never-ending evolutionary process that has no final destination.

Does this approach work for delivering custom projects?
--------------------------------------------------------

Most of the time, no.

You need to have a client who is your partner and you both want to deliver the best possible solution, even if you know up front that you won't find out what it is until the end.

In our business it's called agile development, and these principles build on that. On the other hand, I've observed that agile development as I know it doesn't suit me directly, and I like to come up with bent alternatives.

With agility, I see a lot of the principle of commitment in terms of who solves what within a particular sprint. It makes more sense to me to build the environment so that you have a huge backlog of tasks that get solved based on what is needed most right now, or I have the mental capacity to do. When I'm on the road, for example, I tend to tackle more developmental thinking tasks that I can do outside without a computer. When I'm in the office again, I tackle more algorithm and maths intensive tasks because I can fully focus and invest a lot of mental energy.

I don't recommend this principle if you're setting up a brand new e-shop or your business is going bust. You need a stable environment that runs itself, and you'll just be jeweling. But at the same time, you know that if you do nothing, your stable environment will end one day.
