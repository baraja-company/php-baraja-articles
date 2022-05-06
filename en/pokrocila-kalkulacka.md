Calculator in PHP: processing a mathematical expression as a string
===================================================================

> id: e6798758-031d-4e7c-b24e-1f77cf61558d
> slug:
> 	cs: pokrocila-kalkulacka
> 	en: calculator-in-php-processing-a-mathematical-expression-as-a-string
> 
> publicationDate: '2020-02-16 17:07:38'
> mainCategoryId: c2134b23-9b10-46b3-aa54-e3996707255e
> sourceContentHash: '63f5336bf2bbabe5312121b57e1ce34e'

Imagine that you are faced with the task of processing a simple mathematical example that a user enters as a text string, for example, into a search box. Typically, the user wants to perform a simple numerical operation with numbers. This article describes the thought process and specific instructions on how to do it.

Naive implementation
-------------------

For a long time I wondered if a simple mathematical expression could be processed by some trick to make the code as short as possible... and after many years I actually have the solution.

Consider the solution given **only as an example**, because it is **extremely dangerous** and a dishonest user can easily underline the string, which for example will delete the whole application or steal the database!

```php
// User query
$query = '5 + 3 * 2';

// Process the expression as regular PHP code
eval('$result = @(' . $query . ');');

// Extract a variable with the solution to the expression
echo $result; // prints 11
```

The trick is that the <a href="/function-eval">eval()</a> function executes the string as if it were in the context of PHP code. It's crazy, but it works. The wrapper suppresses error messages.

Dealing with more complex inputs
--------------------------

In addition to the fact that **processing expressions via eval() is extremely dangerous**, it also doesn't provide an eloquent enough syntax to suit everyone. If the user makes even a single syntax violation, the entire expression will not be able to be processed.

Therefore, the solution is to **understand** and correct the user query according to the formal aspect first (called normalizing to the canonical form), and then pass and process it further.

I have programmed [QueryNormalizer](https://github.com/mathematicator-core/engine/blob/master/src/QueryNormalizer.php) for exactly this task in the past.

The processing itself is a very challenging task, because you need to understand the different contexts correctly. For example, that parentheses denote nested blocks and must be evaluated recursively. For example, the expression `5+2^(1+3/2)` cannot be solved straightforwardly because the fraction must be solved first, added to the number in the parentheses, then solved for the integer power, and finally added at the base level.

To even be able to meet this demanding requirement, the expression can no longer be treated as an ordinary string and we need to **take the level of abstraction**. In essence, mathematics is a kind of language that describes relationships between operations and numbers, because we have to deal with operator priorities, different meanings, contexts, recursion, and even data types. This is where the **query tokenization process** comes in.

> I've been working on the problem of math tokenization since 2015 and have written several different parsers since then.

The best of these, which currently powers the new Mathematicator, is [available opensource on GitHub](https://github.com/mathematicator-core/tokenizer).

The point of tokenization is to **parse a string**, split it into groups of smaller strings of known types, and then convert those into objects (data types). The converted array of objects is then converted by **clever logic into a binary tree** that can describe dependencies and recursion. This is a very demanding process because there are hundreds of possible scenarios and users can be very creative in their querying.

The main advantage of the token array is that it can be very easily passed to the next layer, which for example [performs the actual computation](https://github.com/mathematicator-core/calculator), or [redraws the tree into LaTeX](https://github.com/mathematicator-core/tokenizer/blob/master/src/TokensToLatex.php).

The usage can look elegant like this:

```php
$tokenizer = new Tokenizer(/* some dependencies */);

// Convert math formula to an array of tokens:
$tokens = $tokenizer->tokenize('(5+3)*(2/(7+3))');

// Now you can convert tokens to a more useful format:
$objectTokens = $tokenizer->tokensToObject($tokens);

dump($objectTokens); // Return typed tokens with meta data

// Render to LaTeX
echo $tokenizer->tokensToLatex($objectTokens);

// Render to debug tree (extremely fast):
echo $tokenizer->renderTokensTree($objectTokens);
```

Viewing procedures
-----------------

A significant number of users will appreciate it when **the procedure** is displayed when the program has done the calculation. This is actually useful for the programmer as well, because at least he can easily find out where there is an error in the calculation and correct the algorithm accordingly. When you combine all this with machine learning based on automated tests, you get something amazing.

Look at how `QueryNormalizer` was able to understand your query, pass the data to the tokenizer, it rendered the query into LaTeX according to it and then passed the object tree to the calculator which returned the overall result.

Příklad: [5+2^(1+3/2)](https://mathematicator.com/search/5%2B2%5E%281%2B3/2%29).

The representation of the procedure is implemented by the calculator traversing the input tree and evaluating one rule at a time according to the tokens and rules it contains. When any rule is evaluated, it puts the step information into an array. Occasionally, a step may turn out to be wrong and we have to go back and take a different path in the calculation, but there is quite a lot of magic behind that, which will remain hidden for now and you can study it in the implementation.

Conclusion
-----

The above procedure describes how to elegantly handle mathematical expressions where we have numbers, operations and relationships with them. This approach cannot, for example, modify expressions or solve equations, but we'll look at that next time.

*If you have other ideas on how to process math efficiently, I'd be happy to hear from you.*
