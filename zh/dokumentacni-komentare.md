纪录片评论，捷克语还是英语？
==============

> id: e5f1bf13-ab9e-4a70-a95c-77fb50aa4878
> slug:
> 	cs: dokumentacni-komentare
> 	zh: ji-lu-pian-ping-lun-jie-ke-yu-hai-shi-ying-yu
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1'
> sourceContentHash: a33af5a14338e9895fe87087362bd476

编写文档需要时间，而且往往没有人在你之后读它，所以直接在源代码中写注释是一个好的做法。然而，大量的文字不必要地使代码变得杂乱无章，然后可能无法同时在显示器上显示，再次降低其可读性。

因此，任何代码都应该是**不言而喻的，也就是说，当阅读它时，应该立即清楚它的作用，而且它应该只做一件事。

例如，如果一个函数被命名为 "getUserProfileById($id)"，那么这样的函数是做什么的，以及我们可以期待什么样的返回值，就非常清楚了。这就是为什么注释经常被用来只描述输入和输出参数+对原理的简短文字解释（如果有更复杂的事情发生）。当代码是为多人准备的，礼貌的做法是包括作者和创建日期。

```php
/**
 * 如果传递的值是一个有效的IPv6地址，则返回TRUE。
 * 替代正则表达式。
 * /^(((?=(?>.*?(::))(?!.+\3)))\3?|([\dA-F]{1,4}(\3|:(?!$)|$)|\2))
   (?4){5}((?4){2}|((2[0-4]|1\d|[1-9])?\d|25[0-5])(\.(?7)){3})\z/i
 * @作者Jan Barasek
 */
function isIpV6(string $value): bool
{
   if (str_contains($value, ':')) {
      return (bool) filter_var($value, FILTER_VALIDATE_IP, FILTER_FLAG_IPV6);
   }

   return false;
}
```

从注释中可以看出，该函数希望输入一个IPv6地址，并根据验证的情况返回一个布尔值`TRUE`或`FALSE`。

bootstrap注解用于使用标准化的键来表示数值，许多编辑器可以进一步处理，例如，在调用函数时提示参数，或者自动传递依赖关系。

直接在代码内的注释只在特殊情况下使用。如果代码需要一个内部注释，这通常表明它应该被分解成多个较小的部分，这些部分将解决自己的那部分程序。

语言
--------------

习惯上总是用英文来命名类、方法、函数和变量（以使大量的程序员能够阅读代码），键是相对标准化的，有统一的语法，所以语言在这里也不重要，而对于帮助文本，则取决于项目的用法。如果是一个稳定的团队的小项目，英语不一定是个问题，相反，至少每个人都能完全理解描述的内容。

使用注释的动机
-------------------

特殊字符（呼号）可以被编辑器以外的其他工具注意到，所以很有用，例如，把它的测试（在什么输入我期待什么输出）直接放在函数定义的上方，这样就不会忘记写测试的地方，同时每个程序员都能立即知道这个函数是干什么的。

这里有一个测试定义的小例子（它只是一个符号的概念，并不是一个具体的测试工具）。

```php
Dosadí hodnoty a vynutí jejich dump do prohlížeče / konzole:
@test Název I---> $limit => {1-{5-8}}, $city => {Praha|Kladno|Brno|Plzeň} O---> [DUMP]

Vygeneruje náhodný hash o délce 6 znaků a ověří, zda je hlavička odpovědi jakákoli, kromě 201:
@test Název I---> $hash => {a-z0-9|len:6} O---> [HEADER:***|!HEADER:201]

Vygeneruje dvojici čísel a na výstupu ověří jejich součet:
@test Sčítání čísel I---> $a => {1-5}, $b => {7-12} O---> ($return == $a+$b)

Načte náhodný článek s ID mezi 1 až 30, ověří jeho délku nebo neprázdnost:
@test Získání textu článku I---> $id => {1-30} O---> (strlen($return) > 64 || $return != NULL)

Vygeneruje náhodnou IP adresu a ověří, zda je z České republiky:
@test Geolokace I---> $ip => {1-255}.{1-255}.{1-255}.{1-255} O---> ($return['国家'] == 'EN')

Pokusí se načíst článek s vysokým ID a při testování mrkne na modifikátory (filtry):
@test Neexistující článek I---> $id => {1000000+} O---> [HEADER:4**:3**|NOCONTENT]

Vrátí počet rohů jednorožce:
@test Jednorožec I---> $maRohy => TRUE O---> 1

Kolik prstů ukazuji?
@test Prsty I---> $ukazuji => {0-10|NAME:prsty} O---> ($return == {*|NAME:prsty})
```

其中一般会写，比如说，根据规则。

```php
@test <název_testu> I---> $<proměnná> => <hodnota> O---> [<modifikátor>:<hodnota>] (<výraz_platnosti>)
```

在测试里面，我使用了一个使用正则表达式的随机值生成器。
这里有一些例子。

```php
{<hodnota><směr_nerovnosti>}           {500+}  {250-}   {-200-(-50)}
{<začátek>-<konec>}                    {0-5}   {a-z}   {a-f0-9}
{<Hodnota1>|<Hodnota2>|<Hodnota3>}     {Praha|Kladno|Brno}
{<výraz>|<modifikátor>:<hodnota>}      {a-z0-9|len:6}  {*|len:6}
{*|<modifikátor>:<hodnota>}            {*|randomWord:5}
```
