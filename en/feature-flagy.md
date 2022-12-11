Feature flags / feature on/off switches
=======================================

> id: '4209c32f-655c-46fa-9090-e5ec3883ea61'
> slug:
> 	cs: feature-flagy
> 	en: feature-flags-feature-on-off-switches
> 
> publicationDate: '2022-12-11 15:00:00'
> mainCategoryId: '28a2aef7-7490-43a9-add7-80d8a051f8a9'
> sourceContentHash: '1e32c312eab2ca16539cdb7431e2dda9'

When developing a more complex application, you will appreciate the ability to develop more features up front, distribute them with the next version of your software, and enable the feature later.

This is exactly what feature flags were created for. This article will show you how to use them.

Basic implementation
---------------------

Feature flags are basically a very simple concept of calling a single function/method that decides if a new feature is active.

For example:

```php
echo '<h1>Weather apps</h1>';
echo 'Today it is:' . getWeather();

if (feature('map')) {
   echo 'Map:' . getMap();
}
```

To check the availability of a particular news item, the `feature()` function is called, which decides whether it can allow or ignore the particular feature based on the call name.

Implementation of the decision logic
-------------------------------

Decision logic is often complex. For example, you can only run a specific function from a specific date, or for users in a specific group. For example, I often test the deployment of a new feature on, say, 5% of users in this way so that it doesn't affect everyone at once.

For example, when developing corporate sotfware, this is how we run advertising campaigns and discounts that are valid from a certain date.

If a particular new feature breaks, it is possible to simply disable it with a feature flag for users, and enable it for a group of developers who will test it and bring a fix, for example.
