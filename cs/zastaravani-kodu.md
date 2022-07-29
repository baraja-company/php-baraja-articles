Zastarávání kódu - jak udržovat kompatibilitu
=============================================

> id: "7837ee7e-a150-47bf-a338-2f4c03d5bbed"
> slug:
> 	cs: zastaravani-kodu
>
> publicationDate: "2022-07-29 17:10:00"
> mainCategoryId: "3666a8a6-f2a3-405d-8263-bd53c4301fb3"

Při vývoji velkých systémů (například korporátní aplikace, sdílené balíky softwaru, knihovny, ...), kde spolu komunikuje větší množství vrstev a vývojářů, vzniká problém, jak řešit vydávání nových verzí kódu.

Pojďme se podívat například na situaci, kdy chceme vyvíjet sdílený Composer balík pro komunitu vývojářů.

Sémantické verzování
--------------------

Ještě před vyřešením problému zpětné a dopředné kompatibility musíme vyřešit způsob, jak evidovat změny v softwaru. V současné době (2022) je nejlepší způsob verzovat veškeré změny do systému Git. Repositář se softwarem lze sdílet například přes platformu GitHub nebo GitLab. Každá změna softwaru má svůj jednoznačný identifikátor, který označuje jednotlivé commity a popisuje, co se reálně stalo.

Při vývoji knihoven se mi osvědčila následující strategie:

Na začátku vývoje se vytvoří initial commit ve větvi `master` (nebo `main`), kam se commitne základní souborová struktura.

Pro každý nový požadavek se vytvoří samostatná větev z masteru, ve které se pracuje. Až je změna připravena, pošle se žádost o merge do masteru formou `Pull requestu`. Nad požadavkem proběhne code review a pokud je vše v pořádku, změna bude začleněna do Masteru.

Pokud větev obsahuje zpětně nekompatibilní změnu (BC break, z anglického `Back Compatibility Break`), musí to být patřičně označeno. O způsobu označení BC breaků pojednávají další kapitoly.

Produkční verze knihovny je poté označována za pomoci tagů, které mají následující strukturu (vychází z principu **Sémantické verzování 2.0.0**):

Číslo verzí zapisujeme ve formátu `MAJOR.MINOR.PATCH`. Navyšování jednotlivých čísel verzí probíhá následovně:

- `MAJOR` ― když nastala změna, která není zpětně kompatibilní s ostatními (API)
- `MINOR` ― když se přidá funkcionalita se zachováním zpětné kompatibility
- `PATCH` ― když se opravila chyba a zůstala kompatibilita

Pomocí předběžných verzí a přidáváním metadat je možné upřesnit informace. Např.: `1.0.0-alfa`, `1.0.1-beta+2`.

Další informace o sémantickém verzování si můžete přečíst na oficiálním webu: https://semver.org.

Zpětná a dopředná kompatibilita
-------------------------------

Při návrhu softwaru je potřeba vždy myslet na **zpětnou kompatibilitu** (nové funkce a změny musí být kompatibilní se starým kódem) a v některých případech ještě na **dopřednou kompatibilitu** (současné funkce musí být kompatibilní s budoucí změnou rozhraní).

Vyřešit oba úkoly správně je velmi náročné. Ne vždy je možné provést změnu, aniž by se neporušila kompatibilita.

Při provádění úprav je potřeba vždy postupovat po krocích a nechávat uživatelům dostatek času na změny reagovat.

Následující kapitoly popisují, jak o tom přemýšlet.

Fáze 1: Označení funkce jako zastaralé
--------------------------------------

Základní typ ohrožení kompatibility je odebrání nebo přejmenování funkce, která v minulosti existovala. Nejčastěji to je z důvodu, že se změnily argumenty, které funkce přijímá, nebo jde o starou logiku, která by nově měla být řešena jinak.

V první fázi by se měly staré části kódu označit za zastaralé, ale nijak neměnit.

V PHP na to existuje anotace `@deprecated`, kterou je vhodné psát přímo nad metody, funkce, properties, proměnné, konstanty a obecně veškerý zastaralý kód.

Dobrá praktika také je napsat důvod, proč je konkrétní věc zastaralá a jak bude v budoucnu změněna. Například uvést název nové funkce nebo způsobu použití.

Reálný příklad označení zastaralého kódu: Konstanty budou odebrány, lepší je použít vestavený Enum (BC break z důvodu přechodu na novější verzi PHP):

```php
class OrderNotification
{
	/** @deprecated since 2022-05-24, use enum OrderNotificationType */
	public const
		TYPE_EMAIL = 'email',
		TYPE_SMS = 'sms';
```

Anotace `@deprecated` způsobí pouze tiché upozornění pro IDE (vývojářský nástroj) a kompilační nástroje. Nic nerozbíjí.

Fáze 2: Provolání nové metody / logiky
--------------------------------------

V druhé fázi nahradíme starou implementaci za novou, ale ve staré implementaci použijeme nový způsob. Pomůže to zachovat rozhraní kompatibilní, aniž by si toho uživatel všiml.

Příklad: Metoda je zastaralá, protože místo ní vznikla nová statická služba. Protože ji může někdo používat, tak je pouze označena jako zastaralá a vnitřně provolává novou implementaci. Vývojář může obecně předpokládat, že bude metoda v budoucnu úplně odebrána.

```php
/** @deprecated since 2021-09-11 use Ip::get() instead. */
public static function userIp(): string
{
	return Ip::get();
}
```

Fáze 3: Změna anotací pro statickou analýzu
-------------------------------------------

Pokud používáte statickou analýzu typu PhpStan (velmi doporučuji!), tak je dobrý nápad před reálnou změnou datových typů nejprve přepsat PHPDoc anotace. Statická analýza uživatele upozorní, že se něco rozbilo, ale runtime zůstane nedotčen.

Fáze 4: Vyhození notice
-----------------------

Ve čtvrté fázi se provolává nový způsob použití metody, a zároveň se vyhazuje chyba úrovně `notice` (poznámka). Aplikace stále funguje, jenom se do systémového logu začíná postupně ukládat informace, že je nějaká funkce zastaralá a bude změněna nebo odebrána. Na tento typ změny už budeme aktivně upozorňovat. Vývojář uvidí chyby při vývoji nebo kompilaci.

```php
/** @deprecated since 2021-05-01, use UserMetaManager instead. */
public function getMeta(int $userId, string $key): ?string
{
	trigger_error(__METHOD__ . ': This method is deprecated, use UserMetaManager instead.');
	return $this->userMetaManager->get($userId, $key);
}
```

Fáze 5: Vyhození výjimky
------------------------

Před úplným odebráním metody doporučuji vyhazovat některou z fatálních výjimek. Důležité to je zejména z toho důvodu, že bude aplikace úplně zastavena a chybu nelze ignorovat. Na rozdíl od úplného odebrání kódu bude uživatel upozorněn, co se reálně stalo a může chybu jednoduše opravit.

Fáze 6: Úplné odstranění kódu
-----------------------------

V poslední fázi dojde k úplnému odstranění starého kódu. Pokud někdo z uživatelů neopravil závislosti, dojde k rozbití jeho aplikace.

Závažné BC breaky v citlivých místech je potřeba provést vždy až v následující `MAJOR` verzi a je potřeba na ně minimálně o jednu `MAJOR` verzi dříve upozorňovat vyhozením notice. Pokud to neuděláte, aktualizace knihovny bude extrémně náročná.
