Multi-year form
===============

> id: '2abacddd-8a6b-4d25-a387-603fc7abc333'
> slug:
> 	cs: vicekrokovy-formular
> 	en: multi-year-form
> 
> publicationDate: '2019-09-16 09:30:19'
> mainCategoryId: a23332c0-a233-4093-abd7-85b1b00a383b
> sourceContentHash: '69cb85c856478d588b96e9db9e695d97'

Sometimes we need to split the form into several parts (pages), process them separately and then assemble them into one result.

This article describes methods and design patterns to do this.

> **Note:**
>
> The issue of splitting a form into multiple steps is very complex, especially if you want to do it well. I have encountered many approaches in my lifetime, which I will discuss here. Some approaches look appealing, but are naive and only work in specific cases. For each approach, I describe when it makes sense and which approaches don't make sense.

Design of a theoretical solution
-------------------------

Usually the goal is to get the basic data from the first form on the first page, validate it, then save it "somewhere" and display the next page.

When the user gets to the last page, the overall form needs to be submitted and the inputs processed.

At each step, it's important to always carefully validate all the data and allow the user to skip back through the pages at will so that they can correct the data back when they encounter an error. In addition, if the form is to be rendered conditionally based on the data already obtained, this is a very demanding process.

Implementation of the forms themselves
--------------------------------

We can either implement the individual forms ourselves in pure HTML and then handle the processing in PHP, or use ready-made solutions such as <a href="https://doc.nette.org/cs/3.0/forms">Nette forms</a>.

> **Example from life:**
>
> Very often, novice programmers email me and ask seemingly simple questions for which there is a ready-made solution. For example, specifically about form processing in PHP.
>
> I always recommend to skip manual processing altogether and use a ready-made solution instead. In reality, it is very complicated to correctly implement, for example, validation of the entered email and that 2 passwords within 2 fields match, while we want to redirect the user back to the pre-filled form according to his data and with error messages in case of an error.
>
> Because people **don't know they don't know they don't know** and so instead of investing 1 hour of time in learning a ready-made solution to 99.99% of problems, they prefer to choose their own solution, which they spend dozens of hours debugging, and there are still cases where forms don't work, throw errors, have security vulnerabilities, and don't protect the data entered.

So the goal of this step is to implement several pages at different URLs that will contain blank forms.

I recommend to implement each form independently of the others (atomically) and handle the state passing on a different application layer. The reason is that each form will in practice handle data validation differently, write its outputs differently, handle errors differently, and we will probably want to extend or change it over time, so we don't need to know the context of the whole process and change dozens of sites to do so.

State transfer
---------------

When processing the first form, we want to first validate the data received and if it is correct, then redirect the user to the second step. It is a good idea to handle the redirect as an HTTP redirect, because it can easily happen that the data is not valid, in which case we want to return the user to the first form and not to the next step.

We can basically store states in 4 ways:

**Not recommended:**

- **Don't store anywhere** and completely transmit in the URL. This has the disadvantage that the user can change the data already sent in the URL once and thus spoof the input. Alternatively, we can disclose sensitive information such as passwords in the URL.
- **Add continuously to <a href="/sessions">sessions</a>**, i.e., incrementally insert newly acquired data by key into the field. This has the disadvantage that if the application makes an error, the user is stuck with the sessions and is unable to resolve the error in any way (other than deleting the cookies, which is extremely difficult for most people), plus with an unfinished form there is a risk that the data will remain pre-populated and can be seen by someone else. However, a much worse case occurs if the session has a very short validity (say 5 minutes) and the user loses the data from the first step when filling in the last step... this can get pretty annoying then.

**Recommended:**

- **Depositing into database and passing the identifier**. The first time the form is submitted, we store all the collected data in a database table and generate a random identifier (say a 10 character long string) to pass between pages as a parameter. The advantage of this is that when processing any form, we can write the newly retrieved and validated data directly into the table, and in case of failure, we have physical backups of the parsed forms and can act on them. For example, when an order is incomplete, we can send an email to the user that they have not completed it and increase the chance of a sale.
- The **Save to currently logged in account** works exactly the same as forwarding via ID, except instead of using a random identifier (token) we use a session to the ID of the currently logged in user (if there is one). The advantage is that we can show the pre-populated data to the user arbitrarily in the future.

Conclusion
-----

None of these solutions is perfect or the only correct one. I myself combine multiple approaches when working on multi-step solutions. Typically, for example, I solve a shopping cart as a database table, to which I assign the data I have already collected and bind it either to a user (if he is logged in) or to a session (if he is not logged in and we don't know each other yet).
