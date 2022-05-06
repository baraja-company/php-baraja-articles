Design patterns in PHP
======================

> id: c22b3e54-bb66-4196-a998-f4b62b9308b2
> slug:
> 	cs: navrhove-vzory
> 	en: design-patterns-in-php
> 
> publicationDate: '2020-02-13 10:32:29'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '9ee32f0a3ce55bad52725f45fcd1c041'

Design patterns are ways of thinking about programming.

They provide a collection of advice, ready-made practices, best-practices and insights into development. For each programming paradigm and task type, there are certain design patterns that are best suited.

Benefits - why use design patterns?
---------------------------------------

In programming, solving certain types of problems is repetitive, so it makes sense to choose one method to solve these problems and keep repeating that method.

The main benefit arises especially during team development, when everyone knows how the application will be developed (according to which design pattern) and they just apply it. This then eliminates the unnecessary tens of hours of debugging strangely written code and trying to understand the principles the author intended.

MVC - an example of using a design pattern
--------------------------------------

My favourite design pattern is `MVC` (from `Model View Controller`), which says that the application is divided into 3 independent layers that call each other sequentially and pass data to each other.

For example, when rendering a page, it might look like it first decides what type of page it is (for example, a category detail), so it calls `CategoryController` with the `detail` method.

A concrete example (I'm simplifying a lot):

```php
class CategoryController
{
    public CategoryManager $categoryManager;

    public function actionDetail(string $id): void
    {
        $this->template->id = $id;
        $this->template->category = $this->categoryManager->getById($id);
    }
}
```

> **Note:**
>
> This is just a sample code that explains the principle of the `MVC` design pattern.
>
> In a real implementation, we would have to further figure out how to, for example, get an instance of `CategoryManager` and how to pass it to property. Typically, `Dependency injection` is used for this type of task.

Before rendering the page for the category detail, the `CategoryController` is first called to receive the actual request (i.e., we are rendering the category detail with a certain ID, which is obtained by the router, for example, in the URL), get the data (querying the corresponding `Model`) and pass the final data to the template for rendering.

The huge advantage of this principle is that we can write many models (application logic) that is independent of the way the data is presented (the template), resulting in reusable code. In fact, if we want to use the `CategoryManager` in another project, we simply pass the data through the `Controller` in a specific way, which will be rendered according to the template defined by the project itself, while the application logic remains the same and nobody cares because the software layer has fulfilled its agreed interface and responsibilities.

> **Practical note:**
>
> The `MVC` design pattern is used by most modern frameworks such as Nette, Symfony, Laravel and others.
>
> We can also encounter `MVC` in mobile app development and other types of software where we need to get data and render it in a template according to the page or view type.

Important design patterns for web development
---------------------------------------

Generally in programming there are many design patterns which are not suitable for web development. This list describes the most important patterns that I use myself and you should be familiar with.

A <a href="/category-design-patterns">complete list of all design patterns</a>, examples of their use and detailed explanations can be found on a separate page.

- **MVC** - The principle of dividing an application into `Model` (application logic and data), `View` (template and data view) and `Controller` (linking `Model` and `View`).
- **Dependency injection** - Instead of creating class instances, we define so-called services that we have automatically injected. The code admits all dependencies, individual parts can be swapped out and we build the application dynamically.
- **Singleton (Singleton)** - Each class has only one instance (I personally use it in combination with `Dependency injection`, where each service has only one instance, which is passed across the application).
- **Lazy Initialization (Delayed Initialization)** - We create an instance of an object when it is first needed.
- **Adapter** - If we need to provide communication between two incompatible classes, the `Adapter` converts data from one type to the other (typically converting native PHP data types to database data types and back again).
- **Command** - Instead of executing a specific task directly (for example, sending an email), we just remember that it was supposed to happen. Another or the same process will then pick up the request and process it. The main advantage is that we can process the application lazily (and therefore very fast), retry failed actions and simply store the parameters of the call. At the same time, this will allow us to receive requests from different clients, via APIs and so on. Sent actions can also be queued, prioritized and possibly cancelled before evaluation.
- **Strategy** - Encapsulates some kind of algorithms or objects to be changed so that they are interchangeable for the client.

There are many more design patterns, these were the most important ones you should know.

Anti-pattern
------------

Some programming development techniques are considered `anti-pattern`, which is the exact opposite of a design pattern. It is usually a technique that produces strange code that cannot be easily debugged, maintained, and behaves `magically`.

A typical example is the use of <a href="/global-variable">global variables</a>.
