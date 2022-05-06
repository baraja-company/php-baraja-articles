Heritability and visibility in PPE
==================================

> id: '71896a55-dcfd-4bb6-9f53-64f22b1514eb'
> slug:
> 	cs: dedicnost-a-viditelnost
> 	en: heritability-and-visibility-in-ppe
> 
> publicationDate: '2020-02-16 22:17:05'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '4584f3dcfdcfdc506d34a37c36fe6dd1'

One of the fundamental properties of Object Oriented Programming is **inheritance** and <a href="/encapsulation">encapsulation</a>. With these features, you will be able to easily build complex application logic while maintaining good implementation readability.

The principle of inheritance
-------------------

Inheritance expresses that the implementation of one class is based on another. In OOP terminology, we talk about **descendant** (the class that inherits) and **ancestor** (the class that we inherit).

In general, inheritance works by the descendant getting all the features of the ancestor, either adopting them exactly as the ancestor had them, or modifying them in its own way, or completely overriding them and using its own implementation.

The use of this approach is very broad, and inheritance is used by a number of <a href="/design-patterns">design patterns</a>.

Real use of inheritance - presenters in an application
--------------------

Inheritance is well suited for designing so-called **presenters**, which is a special kind of class representing the linking logic in the **MVC** design pattern.

For example, let's have a trio of `Homepage`, `Contact` and `Login` pages.

As each page is implemented, much of the logic will be repeated (for example, accepting a request, building the URL, rendering the template, and submitting the resulting HTML). It is therefore convenient to implement a single ancestor with this logic and just use it in descendants.

We start by first defining the ancestor (the class name doesn't matter, I'm using a convention from the Nette framework):

```php
abstract class BasePresenter
{
   public function link(string $route, array $params = []): string
   {
      // implementation of the method for building the URL
   }

   public function renderTemplate(string $path, array $params = []): string
   {
      // template rendering logic
   }
}
```

When defining the class, I used the new `abstract` keyword to say that the `BasePresenter` class is abstract. This means that we can't create an instance of it, and we only need to use it so that another class inherits and implements it. Abstraction has other useful advantages, which we'll discuss later. A class doesn't have to be abstract to be inheritable - it's just one of the possible settings.

Now we can implement a second class, for example `HomepagePresenter`:

```php
final class HomepagePresenter extends BasePresenter
{
   public function run(): void
   {
       // rendering logic
       $this->renderTemplate('homepage', [
          'contactLink' => $this->link('Contact:default'),
       ]);
   }
}
```

Now you have a working `HomepagePresenter` class. Note that the class is `final`, which means that it can no longer be inherited, guaranteeing that the methods will be used exactly as we specified them.

When we implemented the class, we created a new `run()` method that only `HomepagePresenter` can handle. Inside the method we call the `renderTemplate()` method and `link()`, which the class does not contain. This doesn't matter though, because the `extends` keyword tells us where the methods are to be inherited from, so those are used.

Thanks to inheritance, we were able to achieve code reusability because once written, methods can be used in multiple places.

Overriding the implementation of a specific method
------------

Very often it can be useful to override the behavior of a particular method during inheritance. For example, if we wanted to change the behavior of the `link()` method from the previous example in `ContactPresenter`, it would look like this:

```php
final class ContactPresenter extends BasePresenter
{
   public function run(): void
   {
      // rendering logic
      echo $this->link('Homepage:default', []);
   }

   public function link(string $route, array $params = []): string
   {
      return 'https://baraja.cz';
   }
}
```

To override the implementation, simply redefine the method in the child and override the method body. The important thing is to meet the interface and implement the same input arguments.

Visibility at inheritance
--------------------------

Sometimes we would like to hide some methods during inheritance and use only internally. Alternatively, only allow them to be used during inheritance and not as a public interface.

In general, therefore, there are some simple visibility rules. We denote methods by `public`, `protected` or `private`, and the visibility rules are as follows:

- Anyone can call `public` methods from anywhere, that is, when creating an instance of both an ancestor and a descendant.
- Only an ancestor or descendant can call `protected` methods, but they cannot be called from the public interface when an instance is created. These are internal methods to achieve inheritance (a convenient application is for example to the `link()` method in the previous example).
- Only the current class can call `private` methods, regardless of the inheritance and public interface settings.

Changing visibility at runtime
----------------------------

In very specific cases, it may be useful to change the visibility of a method at runtime and then call it. This is used, for example, by various Doctrine libraries.

To change the visibility, we then use the native **ReflectionClass** class implemented by PHP itself.
