Flaggor för funktioner / på/av-omkopplare för funktioner
========================================================

> id: '4209c32f-655c-46fa-9090-e5ec3883ea61'
> slug:
> 	cs: feature-flagy
> 	sv: flaggor-foer-funktioner-pa-av-omkopplare-foer-funktioner
> 
> publicationDate: '2022-12-11 15:00:00'
> mainCategoryId: '28a2aef7-7490-43a9-add7-80d8a051f8a9'
> sourceContentHash: '1e32c312eab2ca16539cdb7431e2dda9'

När du utvecklar ett mer komplext program kommer du att uppskatta möjligheten att utveckla fler funktioner i förväg, distribuera dem med nästa version av programvaran och aktivera funktionen senare.

Detta är precis vad funktionsflaggor skapades för. I den här artikeln får du veta hur du använder dem.

Grundläggande genomförande
---------------------

Funktionsflaggor är i princip ett mycket enkelt koncept som innebär att man anropar en enda funktion/metod som avgör om en ny funktion är aktiv.

Till exempel:

```php
echo '<h1>Väderappar</h1>';
echo 'I dag är det:' . getWeather();

if (feature('karta')) {
   echo 'Karta:' . getMap();
}
```

För att kontrollera om en viss nyhet är tillgänglig anropas funktionen `feature()`, som avgör om den kan tillåta eller ignorera en viss funktion baserat på anropsnamnet.

Genomförande av beslutslogiken
-------------------------------

Beslutslogiken är ofta komplex. Du kan till exempel bara köra en viss funktion från ett visst datum eller för användare i en viss grupp. Jag testar till exempel ofta distributionen av en ny funktion på till exempel 5 % av användarna på det här sättet så att den inte påverkar alla på en gång.

När vi till exempel utvecklar företagsprogramvara är det så här vi kör reklamkampanjer och rabatter som gäller från och med ett visst datum.

Om en viss ny funktion går sönder är det möjligt att helt enkelt inaktivera den med en funktionsflagga för användarna och aktivera den för en grupp utvecklare som testar den och tar fram en lösning, till exempel.
