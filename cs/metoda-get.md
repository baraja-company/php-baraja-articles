Získání parametrů z URL metodou GET
===================================

> id: bbf2cb2c-f7f7-4be9-a9cd-960014db0f51
> slug:
> 	cs: metoda-get
> 
> publicationDate: "2019-08-22 20:48:46"
> mainCategoryId: "2a1ef8bc-14aa-438a-87e7-5b3f9643f325"

Však to znáte, máte otevřenou nějakou stránku, sledujete URL a vidíte otazník s nějakými parametry. Ještě nezkušený programátor si řekne, že to jsou samostatné soubory, ale ejhle. Zkuste vytvořit soubor co má v názvu otazník (nejde to). **To je právě důvod, proč vznikl tento článek**.

O co jde?
--------------------------

Vlastně jde o to, že je to jeden soubor, do kterého se předávají proměnné přes URL adresu, takže mám třeba soubor **index.php**, a já mu předám jméno článku: **index.php?clanek=o-php**.

Kód + vysvětlení
--------------------------

Superglobální proměnná `$_GET` obsahuje klíče s parametry z URL adresy

```php
echo $_GET['clanek'] ?? '';
```

Bezpečnost a hranice délky
--------------------------

Metoda GET není bezpečná, proto by se přes ní neměli posílat důvěrné data, jeden z hlavních důvodů je, že je to komunikace nešifrovaná a za druhé se ukládá v historii.

Důvěrná data a nebo prostě všechno by se mělo posílat metodou <a href="/metoda-post">POST</a>. GET se hodí spíše pro furmuláře, kde je dobré parametry ukázat (třeba vyhledávače, stránka s článkem), aby se na tu stránku dalo odkázat.

Délka GETu není neomezená! Na to doplácí spousta začátečníků. Maximální délka se pohybuje kolem 1024 znaků (někde se uvádí 1088). Proto delší texty posílejte <a href="/metoda-post">POST</a>em.
