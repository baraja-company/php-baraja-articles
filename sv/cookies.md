Cookies i PHP
=============

> id: '392dd88b-d2f5-4943-a993-01aaad7ccd32'
> slug:
> 	cs: cookies
> 	sv: cookies-i-php
> 
> perex:
> 	- 'Cookies je krátká textová informace v paměti prohlížeče, kam můžeme v PHP zapisovat a označit si uživatele.'
> 	- En cookie är en kort bit textinformation i webbläsarens minne där vi kan skriva och markera användaren i PHP.
> 
> publicationDate: '2019-09-11 10:18:29'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '78f9aad0278acd51b98179e7d29fe3d8'

> **Varning:** Den här artikeln skrevs för många år sedan och viss information kan vara föråldrad eller felaktig. Tänk på detta när du läser.

Cookies är små bitar av textinformation som lagras i en webbplatsbesökares webbläsare. De överförs alltid med varje sida som laddas på nytt och kan raderas, ändras och läsas av användaren när som helst, så de är inte lämpliga för att lagra personlig information.

*Varning: Om din webbplats använder cookies för att spåra användare eller tillägg från tredje part (t.ex. Facebooks gilla-knapp, Google Analytics trafikmätare, reklambanners) måste du informera användaren om detta.*

> "Ytterligare en anmärkning: din webbplats bör inte innehålla reklam eller mätkoder förrän du har fått samtycke. Och det suger."
>
> -- <a href="https://phpfashion.com/jak-na-souhlas-s-cookie-ve-zkurvene-eu">David Grudl</a>

Läsa ett värde från en kaka
--------------------------

Alla cookies lagras i den superglobala variabeln `$_COOKIE`, som lagrar varje nyckel som en array.

Om vi till exempel har lagrat namnet på den inloggade användaren under nyckeln `user` i kakan kan vi enkelt hämta det:

```php
echo $_COOKIE['användare'];
```

Observera: Cookies finns kanske inte alltid (t.ex. om du är en ny användare). Därför bör vi alltid kontrollera om det finns cookies innan vi lägger upp en webbplats och vid behov erbjuda ett alternativt felmeddelande.

```php
if (isset($_COOKIE['användare']) && $_COOKIE['användare']) {
    echo 'Inloggad användare:' . $_COOKIE['användare'];
} else {
    echo 'Ingen har loggat in.';
}
```

Hämta alla tillgängliga kakor
--------------------------------

Eftersom alla cookies lagras i den superglobala variabeln `$_COOKIE` kan de enkelt listas:

```php
var_dump($_COOKIE);
```

Alternativt kan du gå igenom cykeln och hämta alla nycklar och värden:

```php
foreach($_COOKIE as $key => $value) {
    echo $key . ':' . $value;	// skriva ut nyckeln och värdet
    echo '<br>';				// omsluta linjen
}
```

Lagra värdet i en kaka
--------------------------

Funktionen `setcookie()` används för att lagra data i cookies.

Den första parametern är cookienyckeln, som används för att läsa den från fältet `$_COOKIE`, och den andra parametern är själva datan i form av en sträng.

Med den tredje parametern kan vi (frivilligt) ställa in giltighetstiden för kakan. Tillgänglighetstiden anges som <a href="/date">tidsstämpel</a>, så om vi vill ställa in en cookie med en giltighet på 1 timme från och med nu behöver vi bara skriva `time() + 3600`.

```php
$data = 'Innehåll som vi vill lagra.';

setcookie('TestCookie', $data);
setcookie('TestCookie', $data, time() + 3600);
```

Lagring av större data
-------------------

Cookies är inte lämpliga för att lagra större uppgifter (webbläsare tillåter vanligtvis endast 4 kB och högst 20 cookies att lagras, storleken omfattar även cookienamn, giltighetsinställningar etc.).

Det är bättre att lagra större uppgifter på servern och bara lägga in en identifierare i kakan, så att vi kan se vilken användare den tillhör. Denna metod kallas `$_SESSION` och diskuteras i en separat artikel.

Om du inte nödvändigtvis behöver lagra data alltid synkront på servern kan du använda **<a href="https://jecas.cz/localstorage">localstorage</a>** lagring som finns i javascript. Kapaciteten är i storleksordningen MB och uppgifterna kan inte upphöra att gälla.
