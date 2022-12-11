Feature flagy / zapínání a vypínání funkcí
==========================================

> id: 4209c32f-655c-46fa-9090-e5ec3883ea61
> slug:
> 	cs: feature-flagy
> 
> publicationDate: "2022-12-11 15:00:00"
> mainCategoryId: "28a2aef7-7490-43a9-add7-80d8a051f8a9"

Při vývoji složitější aplikace oceníte možnost vyvinout více funkcí dopředu, ty distribuovat společně s další verzí vašeho software, a funkci povolit až později.

Přesně pro tento účel vznikly tzv. feature flagy. Tento článek ukáže možnosti jejich použití.

Základní implementace
---------------------

Feature flagy jsou v podstatě velmi jednoduchý koncept volání jedné funkce/metody, která rozhodne, jestli je nová feature aktivní.

Například:

```php
echo '<h1>Aplikace o počasí</h1>';
echo 'Dnes je: ' . getWeather();

if (feature('map')) {
   echo 'Mapa: ' . getMap();
}
```

Pro ověření dostupnosti konkrétní novinky se provolává funkce `feature()`, která podle volacího názvu rozhodne, jestli může konkrétní funkci povolit nebo ignorovat.

Implementace rozhodovací logiky
-------------------------------

Rozhodovací logika bývá často složitější. Například můžete konkrétní funkci spustit až od konkrétního data, nebo pro uživatele z určité skupiny. Často takto například testuji nasazení nové funkce třeba na 5 % uživatelů, aby to neovlivnilo všechny najednou.

Při vývoji firemního sotfware takto například spouštíme reklamní kampaně a slevy, které platí od určitého dne.

Pokud se konkrétní novinka rozbije, je možné ji jednoduše feature flagem pro uživatele vypnout, a povolit například jen pro skupinu vývojářů, kteři ji otestují a přinesou opravu.
