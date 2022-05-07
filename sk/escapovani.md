Vynechávanie znakov v reťazci v jazyku PHP
==========================================

> id: '40f9361e-e286-4b5e-a0c0-1f6cda8106af'
> slug:
> 	cs: escapovani
> 	sk: vynechavanie-znakov-v-retazci-v-jazyku-php
> 
> publicationDate: '2019-11-26 11:56:52'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '026284252541bc9c918a87298b9e261c'

Escaping sa používa na písanie znakov, ktoré majú v rôznych kontextoch rôzny význam.

Napríklad chceme do reťazca uzavretého úvodzovkami vložiť ďalšiu úvodzovku. Ako na to?

K dispozícii sú 2 možnosti:

```php
echo "Džínsy Levi's"; // Kombinácia typov úvodzoviek

echo 'Džínsy značky Levi'; // Spätné lomítko uniká
```

Escapovanie je dôležité aj pri zápise premenných do šablóny HTML, kde sa obsah reťazca môže nachádzať v inom kontexte a môže znamenať niečo špeciálne.

Preto napríklad pri výpise HTML kódu (ktorý máme v premennej) musíme ošetriť výpis, inak sa HTML kód spustí.

Napríklad:

```php
$message = 'Ahoj <b>Tommy!</b>';

echo $message; // Omyl!

echo htmlspecialchars($message); // Správne :)
```

Problematika útekov je veľmi zložitá a odporúčam prečítať si článok <a href="https://phpfashion.com/escapovani-definitivni-prirucka">Escaping - The Definitive Guide</a> od Davida Grudela.
