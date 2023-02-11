Urgent repair of overloaded server
==================================

> id: '07c5d1ed-f854-4797-8efe-350a24594762'
> slug:
> 	cs: senior-6-pretizeny-server
> 	en: urgent-repair-of-overloaded-server
> 
> publicationDate: '2023-02-11 14:25:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: f8239dd6b0eee234efc999c0dd3e0aeb

An external monitoring tool will report to you that the average response time of the 5 monitored URLs has doubled in the last 30 minutes. The project is running on a single physical server that is not under your management and is running somewhere in a datacenter. You connect via SSH, start htop, and see that the CPU load is 95% and the memory is long overflowing.

According to git, you know that about a week ago they did a database migration to a new table structure, and a colleague writes in chat that he had to run the migration overnight, because the recalculation of columns and indexes took about 5 hours, during which almost the entire database was locked, and neither INSERT nor SELECT worked.

So the performance problems are probably due to improperly designed indexes, poorly redesigned SQL queries, or large connection pooling. There is no time for a revert, there are 7 thousand users on the site according to Google Analytics, and an outage for 5 hours would mean a reputational risk for the client, and a loss of tens to hundreds of thousands of crowns during that time (it's hard to estimate, the projectionists make up enough). You realize that testing only functionality on a test environment is not enough, and you need to implement a load test as well.

Since this is an important e-commerce store of your biggest client, and you expect that the situation may get worse, you have 30 seconds to make a decision.

How do you proceed?

1. You do nothing. The site will be slower, but as long as the server can handle it, I guess it's okay...
2. You'll try to prepare a plan to revert the DB structure, and run it as soon as possible.
3. You migrate the site to a more powerful server (but that means expeditiously raising the price to the client, and then waiting maybe 6 hours for the migration to complete, because you have hundreds of GB of database tables).
4. You find out which SQL queries and which pages take the most time, and simply disable them.
5. You run Cloudflare and let it statically check-in what it can.
6. Some other magic (I don't think there's much to come up with)...
