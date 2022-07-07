变量 变量
=====

> id: a0baeb3a-ab6e-4dd9-b1bc-b1872a12ee08
> slug:
> 	cs: promenna-promenna
> 	zh: bian-liang-bian-liang
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '590515cc711ec27cc0f0bfe190c8fbff'

> **警告：**这篇文章是多年前写的，一些信息可能已经过时或不正确。阅读时请牢记这一点。

**变量**的目的不是为了共同部署（它们解决的问题可以用其他方式解决），它们主要是用来使写法更简洁，内存访问更复杂。

请考虑以下例子。

```php
$x = 25;                  //包含25个
$nacitana_promenna = 'x'; //包含 "x"。
$y = $$nacitana_promenna; //包含25个
echo $y;                  //打印出25
```

注意这两块钱紧随其后。在这种情况下，变量$y的值将被加载到具有$nacitana_variable所含名称的变量中。

有点令人困惑，是吧？这就是为什么你最好不要使用变量。
> **注意：**变量是PHP的一个特色，因为有美元符号。在其他语言中，变量名称的开头没有标记任何字符，所以你不能使用变量，因为当它是一个经典的变量和非经典的变量时，会有歧义。
