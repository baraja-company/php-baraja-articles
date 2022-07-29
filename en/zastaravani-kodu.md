Code obsolescence - how to maintain compatibility
=================================================

> id: '7837ee7e-a150-47bf-a338-2f4c03d5bbed'
> slug:
> 	cs: zastaravani-kodu
> 	en: code-obsolescence-how-to-maintain-compatibility
> 
> publicationDate: '2022-07-29 17:10:00'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '727b60c7258fb70e21b5acb445404db0'

When developing large systems (e.g., enterprise applications, shared software packages, libraries, ...) where multiple layers and developers communicate with each other, the problem of how to handle the release of new code versions arises.

Let's look at an example situation where we want to develop a shared Composer package for a community of developers.

Semantic versioning
--------------------

Before solving the backward and forward compatibility problem, we need to figure out how to keep track of changes to the software. Currently (2022), the best way to version all changes is into Git. The software repository can be shared, for example, via GitHub or GitLab. Each software change has a unique identifier that identifies each commit and describes what actually happened.

The following strategy has worked well for me when developing libraries:

At the beginning of development, an initial commit is created in the `master` (or `main`) branch, where the underlying file structure is committed.

For each new request, a separate branch is created from master in which to work. When the change is ready, a merge request is sent to the master in the form of a `Pull request`. A code review is performed over the request and if everything is ok, the change is merged into the master.

If the branch contains a backwards incompatible change (BC break, from `Back Compatibility Break`), this must be marked accordingly. The method of marking BC breaks is discussed in the following chapters.

The production version of the library is then tagged using tags that have the following structure (based on **Semantic Versioning 2.0.0**):

We write the version number in the format `MAJOR.MINOR.PATCH`. The incrementing of the version numbers is done as follows:

- `MAJOR` - when there is a change that is not backward compatible with others (API)
- `MINOR` - when functionality is added while maintaining backward compatibility
- `PATCH` - when a bug is fixed and backward compatibility is maintained

By using pre-releases and adding metadata it is possible to refine information. For example: `1.0.0-alpha`, `1.0.1-beta+2`.

You can read more about semantic versioning on the official website: https://semver.org.

Backward and Forward Compatibility
-------------------------------

When designing software, you should always think about **backward compatibility** (new features and changes must be compatible with old code) and in some cases, **forward compatibility** (current features must be compatible with future interface changes).

Getting both tasks right is very challenging. It is not always possible to make a change without breaking compatibility.

When making modifications, it is always necessary to proceed in steps and allow users enough time to react to the changes.

The following sections describe how to think about this.

Stage 1: Marking a feature as deprecated
--------------------------------------

The basic type of compatibility threat is the removal or renaming of a feature that existed in the past. Most often this is because the arguments that the function accepts have changed, or it is old logic that should be handled differently in the new way.

In the first stage, old parts of the code should be marked as deprecated but not changed in any way.

In PHP there is an annotation `@deprecated` for this, which should be written directly above methods, functions, properties, variables, constants, and generally all deprecated code.

It is also good practice to write a reason why a particular thing is deprecated and how it will be changed in the future. For example, give the name of a new function or method of use.

A real-world example of marking code obsolete: Constants will be removed, it is better to use the built-in Enum (BC break due to the migration to a newer version of PHP):

```php
class OrderNotification
{
	/** @deprecated since 2022-05-24, use enum OrderNotificationType */
	public const
		TYPE_EMAIL = 'email',
		TYPE_SMS = 'text';
```

The `@deprecated` annotation will only cause a silent warning for the IDE (development tool) and compilation tools. It does not break anything.

Phase 2: Calling a new method/logic
--------------------------------------

In the second phase, we replace the old implementation with the new one, but use the new method in the old implementation. This will help to keep the interface compatible without the user noticing.

Example: the method is deprecated because a new static service was created instead. Since someone can use it, it is just marked as deprecated and internally calls the new implementation. The developer can generally assume that the method will be removed completely in the future.

```php
/** @deprecated since 2021-09-11 use Ip::get() instead. */
public static function userIp(): string
{
	return Ip::get();
}
```

Phase 3: Change annotations for static analysis
-------------------------------------------

If you're using a static analysis like PhpStan (highly recommended!), it's a good idea to first rewrite the PHPDoc annotations before actually changing the data types. Static analysis will notify the user that something is broken, but the runtime will remain untouched.

Stage 4: Throwing away the notice
-----------------------

In the fourth phase, a new method is called, and a `note` level error is thrown at the same time. The application still works, it just starts to gradually store information in the system log that a function is deprecated and will be changed or removed. We will now be actively alerting on this type of change. The developer will see errors during development or compilation.

```php
/** @deprecated since 2021-05-01, use UserMetaManager instead. */
public function getMeta(int $userId, string $key): ?string
{
	trigger_error(__METHOD__ . ': This method is deprecated, use UserMetaManager instead.');
	return $this->userMetaManager->get($userId, $key);
}
```

Stage 5: Throwing an exception
------------------------

I recommend throwing one of the fatal exceptions before completely removing the method. This is especially important because the application will be stopped completely and the error cannot be ignored. Unlike removing the code completely, the user will be notified of what actually happened and can easily fix the error.

Stage 6: Complete code removal
-----------------------------

In the last stage, the old code will be completely removed. If any user has not fixed the dependencies, their application will be broken.

Serious BC breaks in sensitive areas should always be made in the next `MAJOR` release and should be pointed out at least one `MAJOR` release earlier by throwing a notice. If you don't do this, updating the library will be extremely difficult.
