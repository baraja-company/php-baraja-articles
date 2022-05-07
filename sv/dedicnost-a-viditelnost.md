Ärftlighet och synlighet i personlig skyddsutrustning
=====================================================

> id: '71896a55-dcfd-4bb6-9f53-64f22b1514eb'
> slug:
> 	cs: dedicnost-a-viditelnost
> 	sv: aerftlighet-och-synlighet-i-personlig-skyddsutrustning
> 
> publicationDate: '2020-02-16 22:17:05'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '4584f3dcfdcfdc506d34a37c36fe6dd1'

En av de grundläggande egenskaperna hos objektorienterad programmering är **ärftlighet** och <a href="/kapsling">kapsling</a>. Med dessa funktioner kan du enkelt bygga komplexa programlogiker samtidigt som du behåller en god läsbarhet.

Principen om arv
-------------------

Arv uttrycker att implementeringen av en klass bygger på en annan klass. I OOP-terminologi talar vi om **descendant** (den klass som ärver) och **ancestor** (den klass som vi ärver).

Generellt sett fungerar arv genom att den efterkommande får alla funktioner från den föregående, antingen genom att ta över dem exakt som den föregående hade dem, ändra dem på sitt eget sätt eller helt åsidosätta dem och använda sin egen implementering.

Användningen av detta tillvägagångssätt är mycket omfattande och arv används i ett antal <a href="/design-patterns">designmönster</a>.

Riktig användning av arv - presentatörer i en tillämpning
--------------------

Arv är väl lämpat för att utforma så kallade **presenters**, som är en speciell typ av klass som representerar länklogiken i designmönstret **MVC**.

Låt oss till exempel ha en trio med sidorna "Homepage", "Contact" och "Login".

När varje sida implementeras upprepas en stor del av logiken (t.ex. att ta emot en begäran, bygga upp URL:n, visa mallen och skicka in den resulterande HTML:n). Det är därför lämpligt att implementera en enda anförvant med denna logik och bara använda den i efterkommande.

Vi börjar med att först definiera anförvanten (klassnamnet spelar ingen roll, jag använder en konvention från Nette-ramverket):

```php
abstract class BasePresenter
{
   public function link(string $route, array $params = []): string
   {
      // implementering av metoden för att bygga upp webbadressen
   }

   public function renderTemplate(string $path, array $params = []): string
   {
      // logik för rendering av mallar
   }
}
```

När jag definierade klassen använde jag det nya nyckelordet `abstract`, som säger att klassen `BasePresenter` är abstrakt. Det betyder att vi inte kan skapa en instans av den, utan bara behöver använda den så att en annan klass ärver och implementerar den. Abstraktion har andra användbara fördelar som vi kommer att diskutera senare. En klass behöver inte vara abstrakt för att vara ärftlig - det är bara en av de möjliga inställningarna.

Nu kan vi implementera en andra klass, till exempel `HomepagePresenter`:

```php
final class HomepagePresenter extends BasePresenter
{
   public function run(): void
   {
       // logik för rendering
       $this->renderTemplate('hemsida', [
          'kontaktLänk' => $this->link('Kontakt:standard'),
       ]);
   }
}
```

Nu har du en fungerande klass `HomepagePresenter`. Observera att klassen är `final`, vilket innebär att den inte längre kan ärvas, vilket garanterar att metoderna kommer att användas exakt som vi specificerade dem.

När vi implementerade klassen skapade vi en ny "run()-metod som endast "HomepagePresenter" kan hantera. I metoden anropar vi metoden `renderTemplate()` och `link()`, som inte finns i klassen. Det spelar dock ingen roll, eftersom nyckelordet `extends` talar om var metoderna ska ärvas från, så de används.

Tack vare arv kunde vi uppnå återanvändning av koden eftersom metoder som en gång skrivits kan användas på flera ställen.

Överordna implementeringen av en specifik metod
------------

Mycket ofta kan det vara användbart att åsidosätta beteendet hos en viss metod under arv. Om vi till exempel vill ändra beteendet hos metoden `link()` från föregående exempel i `ContactPresenter` skulle det se ut så här:

```php
final class ContactPresenter extends BasePresenter
{
   public function run(): void
   {
      // logik för rendering
      echo $this->link('Hemsida:standard', []);
   }

   public function link(string $route, array $params = []): string
   {
      return 'https://baraja.cz';
   }
}
```

Om du vill åsidosätta implementeringen definierar du metoden i underobjektet igen och skriver över metodkroppen. Det viktiga är att gränssnittet är detsamma och att samma inmatningsargument används.

Synlighet för arv
--------------------------

Ibland vill vi dölja vissa metoder under arv och använda dem endast internt. Alternativt kan du bara tillåta att de används vid arv och inte som ett offentligt gränssnitt.

I allmänhet finns det därför några enkla regler för synlighet. Vi betecknar metoder med `public`, `protected` eller `private`, och reglerna för synlighet är följande:

- Vem som helst kan anropa "offentliga" metoder var som helst, dvs. när man skapar en instans av både en förfader och en efterföljare.
- Endast en förfader eller efterföljare kan anropa "skyddade" metoder, men de kan inte anropas från det offentliga gränssnittet när en instans skapas. Detta är interna metoder för att uppnå arv (en praktisk tillämpning är till exempel metoden `link()` i det föregående exemplet).
- Endast den aktuella klassen kan anropa "privata" metoder, oavsett inställningar för arv och offentliga gränssnitt.

Ändra synlighet vid körning
----------------------------

I mycket specifika fall kan det vara användbart att ändra synligheten för en metod vid körning och sedan anropa den. Detta används till exempel av olika Doctrine-bibliotek.

För att ändra synligheten använder vi sedan den inhemska klassen **ReflectionClass** som PHP självt har implementerat.
