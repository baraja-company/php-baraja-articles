PHP funkce str_replace()
================================

> id: bda3769f-76b8-4458-9c75-178979c73cad
> slugCS: funkce-str-replace
> publicationDate: 2019-09-11 10:04:04
> mainCategoryId: 0eeab3a7-a54b-46db-a253-ca6100145648

Dostupnost ve verzích: `PHP 4.0`

Nahradí všechny výskyty vyhledávaného řetězce náhradním řetězcem.

Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$search` | `mixed` |  | Hledaná hodnota, jinak známá jako `$needle`. Pole může být použito k označení více `$needle`. |
| `$replace` | `mixed` |  | Nahrazující obsah za nalezenou hodnotu. Pole může být použito pro více náhrad. |
| `$subject` | `mixed` |  | String nebo pole, ve kterém se bude nahrazovat. Označováno jako ``$haystack`. |
| `$count` | `int` | null |  |


Návratové hodnoty
----------------

`mixed`

Tato funkce vrátí řetězec nebo pole s nahrazenými hodnotami.

/--code php
echo str_replace('https://', 'https://', 'https://google.com'); // https://google.com
\--

Další zdroje
------------

https://php.net/manual/en/function.str-replace.php
