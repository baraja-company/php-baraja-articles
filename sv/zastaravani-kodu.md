Föråldrad kod - hur man upprätthåller kompatibilitet
====================================================

> id: '7837ee7e-a150-47bf-a338-2f4c03d5bbed'
> slug:
> 	cs: zastaravani-kodu
> 	sv: foeraldrad-kod-hur-man-uppraetthaller-kompatibilitet
> 
> publicationDate: '2022-07-29 17:10:00'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '727b60c7258fb70e21b5acb445404db0'

Vid utveckling av stora system (t.ex. företagsapplikationer, delade programvarupaket, bibliotek, ...) där flera lager och utvecklare kommunicerar med varandra uppstår problemet med hur man ska hantera frisläppandet av nya kodversioner.

Låt oss titta på en exempelsituation där vi vill utveckla ett delat Composer-paket för en grupp utvecklare.

Semantisk versionering
--------------------

Innan vi löser problemet med bakåt- och framåtkompatibilitet måste vi ta reda på hur vi ska hålla reda på ändringar i programvaran. För närvarande (2022) är Git det bästa sättet att versionera alla ändringar. Programvaruarkivet kan delas till exempel via GitHub eller GitLab. Varje programvaruändring har en unik identifierare som identifierar varje commit och beskriver vad som faktiskt hände.

Följande strategi har fungerat bra för mig när jag har utvecklat bibliotek:

I början av utvecklingen skapas en första commit i grenen `master` (eller `main`), där den underliggande filstrukturen är committed.

För varje ny begäran skapas en separat gren från master där man kan arbeta. När ändringen är klar skickas en begäran om sammanslagning till master i form av en `Pull request`. En kodgranskning utförs på begäran och om allt är okej läggs ändringen in i masterversionen.

Om grenen innehåller en ändring som är inkompatibel med bakåtkompatibilitet (BC break, från `Back Compatibility Break`) måste detta markeras i enlighet med detta. Metoden för att markera BC-avbrott diskuteras i de följande kapitlen.

Produktionsversionen av biblioteket märks sedan med hjälp av taggar som har följande struktur (baserat på **Semantic Versioning 2.0.0**):

Vi skriver versionsnumret i formatet `MAJOR.MINOR.PATCH`. Förändringen av versionsnumren sker på följande sätt:

- `MAJOR` - när det finns en ändring som inte är bakåtkompatibel med andra (API).
- `MINOR` - när funktionalitet läggs till med bibehållen bakåtkompatibilitet.
- `PATCH` - när ett fel har rättats och bakåtkompatibilitet bibehålls.

Genom att använda förhandsutgåvor och lägga till metadata kan man förfina informationen. Till exempel: `1.0.0.0-alpha`, `1.0.1-beta+2`.

Du kan läsa mer om semantisk versionering på den officiella webbplatsen https://semver.org.

Kompatibilitet bakåt och framåt
-------------------------------

När du utformar programvara bör du alltid tänka på **bakåtkompatibilitet** (nya funktioner och ändringar måste vara kompatibla med gammal kod) och i vissa fall **framåtkompatibilitet** (nuvarande funktioner måste vara kompatibla med framtida ändringar av gränssnittet).

Det är en stor utmaning att klara av båda uppgifterna. Det är inte alltid möjligt att göra en ändring utan att bryta kompatibiliteten.

När du gör ändringar bör du alltid gå fram stegvis och ge användarna tillräckligt med tid för att reagera på ändringarna.

I följande avsnitt beskrivs hur man kan tänka på detta.

Steg 1: Markering av en funktion som föråldrad
--------------------------------------

Den grundläggande typen av hot om kompatibilitet är att en funktion som fanns tidigare tas bort eller byter namn. Oftast beror det på att argumenten som funktionen accepterar har ändrats, eller så är det gammal logik som bör hanteras annorlunda på det nya sättet.

I det första steget bör de gamla delarna av koden markeras som föråldrade men inte ändras på något sätt.

I PHP finns det en annotation `@deprecated` för detta, som bör skrivas direkt ovanför metoder, funktioner, egenskaper, variabler, konstanter och i allmänhet all deprecated kod.

Det är också bra att skriva en motivering till varför en viss sak är föråldrad och hur den kommer att ändras i framtiden. Ange till exempel namnet på en ny funktion eller ett nytt användningssätt.

Ett exempel på hur man i verkligheten markerar att koden är föråldrad: Konstanter kommer att tas bort, det är bättre att använda den inbyggda Enum (BC-avbrott på grund av migreringen till en nyare version av PHP):

```php
class OrderNotification
{
	/** @föråldrad sedan 2022-05-24, använd enum OrderNotificationType */
	public const
		TYPE_EMAIL = 'e-post',
		TYPE_SMS = 'text';
```

Annotationen `@deprecated` kommer endast att orsaka en tyst varning för IDE (utvecklingsverktyget) och kompileringsverktygen. Det bryter inte sönder någonting.

Fas 2: Kalla en ny metod/logik
--------------------------------------

I den andra fasen ersätter vi det gamla genomförandet med det nya, men använder den nya metoden i det gamla genomförandet. Detta hjälper till att hålla gränssnittet kompatibelt utan att användaren märker det.

Exempel: Metoden är föråldrad eftersom en ny statisk tjänst har skapats i stället. Eftersom någon kan använda den markeras den bara som föråldrad och kallar internt det nya genomförandet. Utvecklaren kan i allmänhet anta att metoden kommer att tas bort helt och hållet i framtiden.

```php
/** @föråldrad sedan 2021-09-11 Använd Ip::get() istället. */
public static function userIp(): string
{
	return Ip::get();
}
```

Fas 3: Ändra kommentarer för statisk analys
-------------------------------------------

Om du använder en statisk analys som PhpStan (rekommenderas starkt!) är det en bra idé att först skriva om PHPDoc-annotationerna innan du ändrar datatyperna. Statisk analys meddelar användaren att något är trasigt, men körtiden förblir oförändrad.

Steg 4: Kasta meddelandet
-----------------------

I den fjärde fasen anropas en ny metod, och ett fel på `note`-nivå utlöses samtidigt. Programmet fungerar fortfarande, men det börjar gradvis lagra information i systemloggen om att en funktion är föråldrad och kommer att ändras eller tas bort. Vi kommer nu att aktivt varna för den här typen av ändringar. Utvecklaren kommer att se fel under utvecklingen eller kompileringen.

```php
/** @deprecated since 2021-05-01, använd UserMetaManager istället. */
public function getMeta(int $userId, string $key): ?string
{
	trigger_error(__METHOD__ . ': Den här metoden är föråldrad, använd UserMetaManager istället.');
	return $this->userMetaManager->get($userId, $key);
}
```

Steg 5: Kasta ett undantag
------------------------

Jag rekommenderar att du gör ett av de ödesdigra undantagen innan du tar bort metoden helt och hållet. Detta är särskilt viktigt eftersom programmet stoppas helt och hållet och felet inte kan ignoreras. Till skillnad från att ta bort koden helt och hållet får användaren information om vad som faktiskt hände och kan enkelt rätta till felet.

Steg 6: Fullständig borttagning av koden
-----------------------------

I det sista steget kommer den gamla koden att tas bort helt och hållet. Om någon användare inte har fixat beroendena kommer deras program att gå sönder.

Allvarliga BC-avbrott på känsliga områden bör alltid göras i nästa `MAJOR`-version och bör påpekas minst en `MAJOR`-version tidigare genom ett meddelande. Om du inte gör detta kommer det att bli mycket svårt att uppdatera biblioteket.
