Flagi funkcji / przełączniki włączania/wyłączania funkcji
=========================================================

> id: '4209c32f-655c-46fa-9090-e5ec3883ea61'
> slug:
> 	cs: feature-flagy
> 	pl: flagi-funkcji-przelaczniki-wlaczania-wylaczania-funkcji
> 
> publicationDate: '2022-12-11 15:00:00'
> mainCategoryId: '28a2aef7-7490-43a9-add7-80d8a051f8a9'
> sourceContentHash: '1e32c312eab2ca16539cdb7431e2dda9'

Podczas tworzenia bardziej złożonej aplikacji docenisz możliwość opracowania większej liczby funkcji z góry, rozpowszechniania ich z następną wersją oprogramowania i włączenia funkcji później.

Właśnie po to stworzono feature flag. Ten artykuł pokaże Ci, jak z nich korzystać.

Podstawowe wdrożenie
---------------------

Flagi funkcji są w zasadzie bardzo prostą koncepcją wywołania pojedynczej funkcji / metody, która decyduje, czy nowa funkcja jest aktywna.

Na przykład:

```php
echo '<h1>Aplikacje pogodowe</h1>.';
echo 'Dziś jest to:' . getWeather();

if (feature('mapa')) {
   echo 'Mapa:' . getMap();
}
```

Aby sprawdzić dostępność danej wiadomości, wywoływana jest funkcja `feature()`, która na podstawie nazwy wywołania decyduje, czy może dopuścić lub zignorować daną cechę.

Wdrożenie logiki decyzyjnej
-------------------------------

Logika decyzji jest często złożona. Na przykład można uruchomić określoną funkcję tylko od określonej daty lub dla użytkowników w określonej grupie. Na przykład często testuję wdrożenie nowej funkcji na, powiedzmy, 5% użytkowników w ten sposób, aby nie wpłynęła na wszystkich naraz.

Przykładowo, tworząc sotfware firmowy, w ten sposób uruchamiamy kampanie reklamowe i rabaty, które obowiązują od określonej daty.

Jeśli dana nowa funkcja się zepsuje, można ją po prostu wyłączyć za pomocą flagi funkcji dla użytkowników, a włączyć ją dla grupy deweloperów, którzy przetestują ją i przyniosą poprawkę, na przykład.
