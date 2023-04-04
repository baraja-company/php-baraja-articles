CORS HTTP hlavičky
==================

> id: "7d92356e-0337-4fc9-821a-b90c499a906e"
> slug:
> 	cs: cors
>
> publicationDate: "2023-04-04 12:00:00"
> mainCategoryId: "3666a8a6-f2a3-405d-8263-bd53c4301fb3"

Cross-Origin Resource Sharing (CORS) je bezpečnostní mechanismus, který umožňuje webovým stránkám z jednoho zdroje přistupovat k datům ze serverů s jiným zdrojem. Bez CORS by takový přístup byl zakázán kvůli bezpečnostním důvodům. CORS tedy umožňuje vytvářet webové aplikace, které jsou distribuovány na různých doménách, ale stále jsou schopné komunikovat a využívat zdrojů jiných serverů. V tomto článku se zaměříme na to, jak fungují CORS HTTP hlavičky, jak je možné je použít v API a jak lze ošetřit CORS v PHP.

Zabezpečení prohlížečů
----------------------

CORS je primárně zabezpečení prohlížečů, které chrání uživatele před nebezpečnými požadavky z neautorizovaných zdrojů. Webové stránky mohou použít JavaScript k odeslání požadavků na jiné servery, ale CORS zajišťuje, aby tyto požadavky byly povoleny pouze pro konkrétní webovou stránku.

Jak fungují CORS HTTP hlavičky
------------------------------

CORS funguje na základě HTTP hlaviček, které jsou přidány do HTTP odpovědi serveru. Tyto hlavičky říkají prohlížeči, zda je požadavek povolen a jakým způsobem může být zpracován.

Hlavními hlavičkami CORS jsou:

- `Access-Control-Allow-Origin`: Tato hlavička specifikuje, které zdroje mají povolený přístup k serveru. Pokud je hodnota této hlavičky *, pak je přístup povolen ze všech zdrojů.
- `Access-Control-Allow-Credentials`: Tato hlavička umožňuje serveru vyslat cookie nebo jiné formy autentizace na klienta. Pokud je tato hodnota nastavena na true, pak jsou tyto autentizační informace přijímány.
- `Access-Control-Allow-Methods`: Tato hlavička specifikuje povolené HTTP metody, které jsou povoleny pro požadavek. Například, GET, POST, PUT, DELETE a OPTIONS.
- `Access-Control-Allow-Headers`: Tato hlavička umožňuje specifikovat, které HTTP hlavičky jsou povoleny pro požadavek.

Použití v API
-------------

Použití CORS je důležité při vývoji API, zejména pokud poskytujete veřejné API. Pokud váš server povoluje přístup z jiných zdrojů, musíte povolit přístup všem požadovaným zdrojům pomocí hlavičky Access-Control-Allow-Origin.

Možnosti ošetření CORS v PHP
----------------------------

V PHP můžete použít následující logiku pro ošetření CORS:

```php
public static function setHeaders(): void
{
    if (isset($_SERVER['HTTP_ORIGIN'])) {
        header('Access-Control-Allow-Origin: ' . $_SERVER['HTTP_ORIGIN']);
        header('Access-Control-Allow-Credentials: true');
    }
    if (($_SERVER['REQUEST_METHOD'] ?? 'GET') === 'OPTIONS') {
        if (isset($_SERVER['HTTP_ACCESS_CONTROL_REQUEST_METHOD'])) {
            header('Access-Control-Allow-Methods: GET, POST, OPTIONS');
        }
        if (isset($_SERVER['HTTP_ACCESS_CONTROL_REQUEST_HEADERS'])) {
            header('Access-Control-Allow-Headers: ' . $_SERVER['HTTP_ACCESS_CONTROL_REQUEST_HEADERS']);
        }
        die;
    }
}
```

Tato logika zpracovává požadavky a nastavuje odpovídající HTTP hlavičky pro CORS. Pokud je přijat požadavek `OPTIONS`, metoda nastaví povolené metody a hlavičky a ukončí běh skriptu. Tento kód se obvykle nachází na začátku skriptu.

V případě, že používáte framework, může mít tento mechanismus CORS již implementovaný a nemusíte vytvářet vlastní metodu.

Závěr
-----

CORS je důležitý mechanismus pro webové aplikace, které vyžadují přístup k datům z různých zdrojů. V tomto článku jsme si vysvětlili, jak fungují CORS HTTP hlavičky, které jsou zodpovědné za správné fungování tohoto mechanismu. Dále jsme se zaměřili na to, jak je možné CORS použít v API a jak lze ošetřit CORS v PHP. Pokud se chystáte na vývoj webových aplikací, je důležité porozumět tomuto mechanismu a správně ho implementovat, aby byla zajištěna bezpečnost uživatelů a funkcionalita aplikací.
