The principle of encapsulation in PPE
=====================================

> id: '54968a42-b678-4385-91ac-c13ba96c9b34'
> slug:
> 	cs: zapouzdreni
> 	en: the-principle-of-encapsulation-in-ppe
> 
> publicationDate: '2020-02-16 21:21:35'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '3fbce6cdcbf5f274312496604569d9c2'

One of the main principles of OOP is the **encapsulation principle**, which says that complex problems should be broken down into many small problems that we can solve independently and simultaneously. At the same time, we as users don't care how it happens and the data (internal state) remains isolated.

For example, if we are solving the problem of how to return the result `1.6` based on a user query with the expression `(5+3)*(2/(7+3))`, probably none of us can write a single function or method that solves this problem at once.

**HINT:** A ready-made solution to this type of example is in the article <a href="/pokrocila-kalkulacka">Processing a mathematical expression as a string</a>, but be prepared for the fact that it's not easy.

Encapsulation brings abstraction over objects
-----------------------------------------

With encapsulation, you'll be able to use objects "as a user", that is, call their methods and not worry at all about how they work internally.

Suppose we are dealing with calculating an employee's salary and we want to use an existing class from another programmer to do it. We only need to know the mandatory constructor parameters and we can "just use" the class:

```php
$wage = new WageEmployee(
    25000, // gross wage
    6, // number of years in the company
    10, // number of years of experience
    true // is he a man?
);

echo $wage->getHruba(); // 25000

echo $mzda->getCista(); // 17800
```

The parameters of the object are fictional and do not correspond to how the wage is actually calculated. In particular, the principle is illustrated by the fact that we **only need to know the general public interface** and we don't even need to deal with the **internal state of the object**, not even the **internal implementation** and certainly not **why it works the way it does**. We simply call the `getCista()` method and get the net payoff.

Encapsulation is a design issue
----------------------------

It is important to note that **encapsulation itself is not a feature or syntax of the language**. The fact that a class and application is encapsulated is only a matter of the programmer designing the application and thinking about the code.

Always think of class design like this:

- KISS (keep it simple), keep the interface simple and don't force the user to think unnecessarily. Solve the complex logic for the user and he will be grateful.
- The user of the class (another programmer or you of the future) doesn't need to know the internal logic at all and just the method names and their parameters should be enough.
- If I need auxiliary calculations for the calculation that are of no interest to the user and are only technical, it makes no sense to create a getter for them at all and they should only be calculated internally.
- The class must satisfy the basic properties of the algorithm, in particular that it works in general for any data.
- Publicly available methods should be designed to provide enough information to easily extend the object with new features in the future, so that we can easily compute new data from what we already know.

Keep internal data non-public
-------------------------------

For properties and methods that deal with internal logic, it makes sense to set the visibility as `private`. The main advantage of this is that they won't be called from outside and the user will be forced to use your designed interface, thus protecting the data and the internal state of the object.

For example, let's have an object representing a bank account where we want to post payments and deal with the current balance:

```php
class BankAccount
{
    private int $sum;

    public function __construct(int $startSum)
    {
        $this->sum = $startSum >= 0 ? $startSum : 0;
    }

    public function getSum(): int
    {
        return $this->sum;
    }

    public function pay(int $price): void
    {
        $newSum = $this->sum - $price;
        if ($newSum < 0) {
            throw new \Exception('You don't have that much money!');
        }

        $this->sum = $newSum;
    }

    public function addMoney(int $money): void
    {
        $this->sum += $money;
    }
}
```

Note that the class contains only a single `private` property `$sum`, which contains the current balance.

If we want to get the current balance, there is a `getSum()` method for that, but we have no way to change the new balance value. We can only remove money using the `pay()` method or add money using the `addMoney()` method.

Thanks to this principle, we always know for sure that nobody can break the object.

If the user tries to pay more money than is actually in the account, the `pay()` method will not allow it because it performs a check calculation before overwriting the `$sum` property, and if the balance should be negative (less than zero), an error exception is thrown and the operation stops.

Conclusion
-----

We have demonstrated the basic principle of encapsulation, which allows us to think better about object abstraction and brings a whole new perspective.

Once you get a good grasp of this principle, you'll see that <a href="/proc-use-frameworks">frameworks start to make a huge amount of sense</a>, because they internally encapsulate a lot of cleverness that you can just use.

Next time we'll look at <a href="/dedicacy-and-visibility">dedicacy and visibility</a>.
