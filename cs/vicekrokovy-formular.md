Vícekrokový formulář
====================

> id: "2abacddd-8a6b-4d25-a387-603fc7abc333"
> slugCS: vicekrokovy-formular
> publicationDate: "2019-09-16 09:30:19"
> mainCategoryId: a23332c0-a233-4093-abd7-85b1b00a383b

Občas potřebujeme rozdělit formulář na více částí (stránek), ty zpracovat samostatně a pak z nich složit jeden výsledek.

Tento článek popisuje metody a návrhové vzory, jak na to.

> **Upozornění:**
>
> Problematika rozdělení formuláře na více kroků je velmi složitá, a to zejména v případě, pokud ji chcete udělat dobře. Během života jsem se setkal s mnoha přístupy, které zde rozeberu. Některé postupy vypadají lákavě, ale jsou naivní a fungují jen ve specifických případech. U každého postupu popisuji, kdy dává smysl a které postupy smysl nedávají.

Návrh teoretického řešení
-------------------------

Obvykle je cílem na první stránce získat základní data z prvního formuláře, zvalidovat, pak si je "někam" uložit a zobrazit stránku další.

Až se uživatel dostane na poslední stránku, je potřeba formulář celkově odeslat a zpracovat vstupy.

V každém kroku je důležité všechna data vždy pečlivě zvalidovat a umožnit uživateli libovolně stránky přeskakovat zpět, aby mohl data zpětně opravit, když narazí na chybu. Pokud se navíc má formulář vykreslovat podmíněně podle již získaných dat, tak jde o velmi náročný proces.

Implementace samotných formulářů
--------------------------------

Jednotlivé formuláře si můžeme buď implementovat úplně sami v čistém HTML a pak v PHP řešit zpracování, nebo použít hotové řešení, jako jsou třeba <a href="https://doc.nette.org/cs/3.0/forms">Nette forms</a>.

> **Příklad ze života:**
>
> Velmi často mi píší začínající programátoři e-maily a ptají se na zdánlivě jednoduché otázky, na které existuje hotové řešení. Třeba konkrétně na zpracování formulářů v PHP.
>
> Úplně vždy doporučuji se na ruční zpracování úplně vykašlat a místo toho použít hotové řešení. Reálně totiž správně implementovat například kontrolu platnosti zadaného e-mailu a že se 2 hesla v rámci dvou polí shodují, přičemž chceme uživatele přesměrovat v případě chyby zpět na předvyplněný formulář podle jeho dat a s chybovými hláškami, je totiž značně komplikované.
>
> Lidé totiž **neví, že neví, že neví** a tak místo investice 1 hodiny času do naučení-se hotového řešení na 99.99 % problémů raději volí vlastní řešení, kterým tráví desítky hodin ladění a stejně existují případy, kdy formuláře nefungují, vyhazují chyby, mají bezpečnostní zranitelnosti a neošetřují vložená data.

Cílem tohoto kroku tedy je implementovat několik stránek na různých URL, ve kterých budou prázdné formuláře.

Doporučuji každý formulář implementovat nezávisle na ostatních (atomicky) a předávání stavů řešit na jiné vrstvě aplikace. Důvod je ten, že každý formulář bude mít v praxi jinak řešenou validaci dat, jinak zapisovat jeho výstupy, jinak zpracovávat chyby a pravděpodobně ho budeme chtít časem rozšířit nebo změnit, tak nemusíme kvůli tomu znát kontext celého procesu a měnit desítky míst.

Předávání stavů
---------------

Při zpracování prvního formuláře chceme nejprve zvalidovat získaná data a pokud jsou správně, tak uživatele přesměrovat na druhý krok. Přesměrování je dobré řešit jako HTTP redirect, protože se může jednoduše stát, že data nebudou validní a v tom případě chceme uživatele vrátit zpět na první formulář a ne na další krok.

Stavy můžeme v zásadě ukládat 4 způsoby:

**Nedoporučované:**

- **Neukládat nikam** a kompletně přenášet v URL. To má nevýhodu v tom, že může uživatel již jednou odeslaná data v URL změnit a tím podvrhnout vstupy. Případně můžeme v URL vyzradit citlivé informace, jako jsou například hesla.
- **Ukládat průběžně do <a href="/sessions">sessions</a>**, tj. postupně nově získaná data vkládat podle klíčů do pole. To má nevýhodu v tom, že v případě chyby aplikace se uživateli zasekne sessions a není schopen chybu nijak vyřešit (kromě smazání cookies, což je extrémně náročné pro většinu lidí), navíc při nedokončeném formuláři hrozí, že údaje zůstanou předvyplněné a může je vidět někdo další. Mnohem horší případ však nastane, pokud má sessions velmi krátkou platnost (třeba 5 minut) a uživatel při vyplňování posledního kroku přijde o data z kroku prvního... to pak dokážně pěkně naštvat.

**Doporučované:**

- **Ukládání do databáze a předávání identifikátoru**. Při prvním odeslaném formuláři si veškerá získaná data uložíme do databázové tabulky a vygenerujeme náhodný identifikár (třeba 10 znaů dlouhý řetězec), který budeme předávat mezi stránkami jako parametr. Výhoda je ta, že při zpracování libovolného formuláře nově získaná a zvalidovaná data rovnou zapíšeme do tabulky, přičemž v případě selhání máme fyzicky zálohované i rozepsané formuláře a můžeme se podle nich zařídit. Například při nedokončené objednávce můžeme uživateli poslat e-mail, že ji nedokončil a zvýšit tak šanci na prodej.
- **Ukládání do aktuálně přihlášeného účtu** funguje úplně stejně, jako předávání přes identifikátor, akorát místo náhodného identifikátoru (tokenu) použijeme relaci na ID aktuálně přihlášeného uživatele (pokud příhlášený je). Výhoda je ta, že můžeme uživateli zobrazit předvyplněná data libovolně v budoucnosti.

Závěr
-----

Žádné z uvedených řešení není dokonalé a jediné správné. Sám při zpracování vícekrokových řešení kombinuji více přístupů. Typicky třeba nákupní košík řeším jako databázovou tabulku, do které připisuji již získaná data a ta vážu buď na uživatele (pokud je přihlášen), nebo na session (pokud není přihlášen a ještě se neznáme).
