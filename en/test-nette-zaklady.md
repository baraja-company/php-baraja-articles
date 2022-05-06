Test of basic Nette knowledge
=============================

> id: d137c177-503c-4e84-8709-0e65b0ce6060
> slug:
> 	cs: test-nette-zaklady
> 	en: test-of-basic-nette-knowledge
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '28a2aef7-7490-43a9-add7-80d8a051f8a9'
> sourceContentHash: '66fb122b33aa8ce41158345bb01769ab'

Success threshold: 15 points

*You get 1 point for each question answered correctly. For any incorrectly answered question you get nothing. If the answer is only partial (and it would not be possible to program the thing based on it), the question counts as incorrect (it is not possible to get half a point). If the solution contains a security bug, or a typo in the code, or a typo in the code, the answer is considered incorrect because it would not run.*

-----------

1 Explain the difference between the `for`, `while`, and `foreach` loops. For each, give 1 specific example of its use that clearly shows its main advantage.


2. We have a variable about which we know almost nothing (we only know its name). How can we see its contents? For example, it is called `$data`.


3. Write the following commands below to work with the Git repository:
- Download the latest changes from the server
- tag the `Statistic.php` file
- tag all files in the project
- tag all files in the `cron` directory
- committing changes with the message "My commit"
- sending the commit to the server


4. Let's have a text string in the variable. Give an example of a function to calculate the checksum.


5. Write a code snippet that creates a `delete` action in `Presenter` that accepts the item ID as an integer and deletes a row from the `question` table according to the specified ID. After a successful deletion, it will print the message "Question deleted" and redirect to the `list` action.

Under question for an extra point: If the deletion fails for some reason, it does not throw a fatal error, but also informs the user about it with a message (flash message).

6. When I create a Nette form, it becomes a component. What is a Nette component?

7. I need to create a simple Nette form to insert a record into a `question` table that contains a list of questions. The structure of the table is:

| Column | Properties |
|-----------|----------------------------------|
| id | int(8), unsigned, auto increment |
| question | varchar(255) |
| is_active | tinyint(1), unsigned, default value: 1 |

Create the appropriate form fields to insert a new row into this table. After inserting the record, a FlashMessage must be fired informing about successful inserting of the record + redirection to editing the record (`edit` action).

- Validation that the form field has been filled
- Validation that the question text fits into the varchar according to the table structure
- Validation that a question with this text no longer exists
- Define another custom `group` table that will contain information about the groups. When creating a question, it will then be possible to determine which group the question belongs to. You will need to set up a session between the tables (describe how this is done and how it will be set up).
- What Latte macro do I use to render the form to the template (default render)?

8. Let's have an edit form in `Presenter` which is created as a component. We want to pass in default values from what is in the database, i.e. we need to get the data from the table in some convenient way.
- How will you proceed and what elements in the code do we need?
- How will you pass the identifier of the currently edited item to the form?
- How do you set the default value for the form element?
- How can we verify that a user is trying to edit an item that doesn't exist in the database, and how can we appropriately inform them of this?

9 Consider the following data retrieved from a database (using a regular Nette Database):

```php
$questions = $this->db->questions()->fetchAll();
```

- How do we output the text of all questions as a bulleted list?
- How do we pass the data from the table to the Latte template?
- What Latte macros will we need to list the items? Give a specific implementation of listing the `id` and `name` columns in the format:

	*1024: How are you ?
	*1025: What did you have for lunch today?

10. List an example of at least 3 different form fields that are written in the form:

```php
$form->add(here will be an example);
```

and for each one, explain what it does and what output it returns (data type + example).


11. Let's have a submitted Nette form.
- How do we get all the fields (names and values) sent?
- Give an example of outputting a field named `question`.
- Provide a concrete implementation of code that will traverse the array of values and keys and return a single string containing the total list of keys and values, so that we can, for example, store this string in a database or send it by email (storing and sending is not the subject of the assignment and will not be evaluated).


12. For each condition, decide whether the result is TRUE or FALSE:
- `1 > 0`
- `1 == 1`
- `1 == "1"`
- `1 === "1"`
- `1 == true`
- `1 === true`
- `1 === false`
- `1 == "1" && 1=== true`


13. What data types are we familiar with in PHP?
- Give at least 5 examples of data type names
- For each example, list at least 3 possible values that can be stored in the data type (if the data type cannot store that many possible values, write that)
- For each data type, give a typical example of its use (what is stored in it in practice)
- What is the difference in condition between `==` (two equals) and `===` (three equals)?
- Explain the disadvantage of using `==` in conditions and how specifically `==` solves this problem (example where `==` may fail and `==` saves the situation)


14. Let's have a coordination table (coordinations table) that lists all coordinations between 2 people. One of them organizes the coordination and the other is a guest. Write a database selection that returns all the rows with coordinations that involve me (am I the organizer of the coordination, or am I the guest of the coordination). The table has columns `id`, `id_user_organizer` (organizer id), `id_user_quest` (guest id). My ID is stored in the usual way in `Presenter`.


15. Group of questions about Latte:
- What is Latte?
- What is the difference between `variable`, `macro`, `filter` and `n:attribute`? What is used where?
- How do I create a `DashboardPresenter` reference to a `default` action?
- Listing a table of questions, how do I generate a reference to a specific edit (`QuestionPresenter`, `edit` action) of a question to pass the ID of the currently listed question? Write specific Latte code.

Symbolically written (sample in PHP, translate to Latte):

```php
foreach ($questions as $question) {
   echo $question->id; // question id
   echo $question->question; // question text
}
```

- How to write a fixed HTML space?


16. What are services used for in Nette?
- How is it instantiated?
- What does a service have to do to be used?
- How do I load a service into Presenter?
- For example, consider the `StatisticManager` service, which has a public method `getStatistics()` that accepts no parameters. How do I load this service in Presenter and call the public method `getStatistics()` in the default action and pass the result to the template?
- What is the difference between `object`, `class`, `service`?
- What is `model`, `entity` and `value object`?
- All services have a master manager that knows them and can be picked up in that manager. What is the name of this manager? How do I register a new service into it?


17. Neon
- What are Neon files?
- What types are there and what are they sorted by?
- What do Neon files contain? What data are stored in them?
- Give an example of how to register the following field (the recipe is in PHP, it will need to be translated) so that it can be used as a parameter:

```php
$imageGenerator = [
   "points" => [
      480: [910, 30, 1845, 1150],
      600: [875, 95, 1710, 910],
      768: [975, 130, 1743, 660]
   ]
];
```

- Give an example of registering a service in Neon, passing the `imageGenerator` parameter that we registered in the previous task, so that the service receives it in the constructor and can use it in the service (in the sense of configuration). For the service, provide a sample implementation of the constructor so that the first input parameter is treated as the data type for the array.


18. Objects in general
- What are `method`, `properties` and `constants`? What is the difference between them?
- Both methods and properties have 3 basic accessibility states (`public`, `private`, `protected`), explain the difference and a specific example of usage and who can see what and when.
- I have a class `course` in which there is a private property `currentCourse` in which the current course is stored. How to make the property read only and not write from the outside?


19. When I create tables in a database that are logically related (for example, a table for a user and then a table for his articles), I need to treat that the data will be linked correctly.
- What ensures that the data within the tables is linked correctly in the database?
- What is referential integrity and what is its role in the database?
- What kinds of sessions do we have? What is the purpose of each type?
- What parameters do we set for sessions to handle data deletion or modification in different ways? Give 3 examples and specific uses + a description of how it works.


20. What is the purpose of factories (`OOP design pattern`)?
- Give an example of a use.
- Are Nette services factories?
- What is the purpose of dependency injection?
- What is the difference between `DI` and `DIC`?
