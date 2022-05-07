Podstawy filozofii programowania zorientowanego obiektowo
=========================================================

> id: c38214d9-8c92-4484-9c31-239d716f2545
> slug:
> 	cs: filosofie-oop
> 	pl: podstawy-filozofii-programowania-zorientowanego-obiektowo
> 
> publicationDate: '2020-01-02 22:21:56'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: a3841046b40a039413bacd045f275c68

Programowanie obiektowe to paradygmat, czyli pogląd na to, jak należy programować. Wkrótce sam się przekonasz, że OOP przynosi całkiem zasadnicze uproszczenie wszystkich typowych problemów i trudności, które są wciąż i wciąż rozwiązywane w prawdziwym programowaniu.

Podstawowa idea
-----------------

Podstawową ideą programowania obiektowego jest podzielenie dużej aplikacji (złożonego zadania) na wiele małych części, które możemy rozwiązać elegancko i niezależnie od siebie.

Na przykład, jeśli programujemy system rezerwacji biletów lotniczych, jest to bardzo złożony projekt, który rozwiązuje tysiące spraw. Jeśli uda nam się rozłożyć całą złożoną logikę na wiele warstw i części, będziemy mogli łatwo zrozumieć cały złożony system i niezależnie programować poszczególne podzadania.

Praktyczne zalety OOP i podziału kodu na obiekty
------------------------------------------------

Poza naukowym punktem widzenia istnieje wiele praktycznych powodów, dla których warto stosować OOP:

- Aplikacja utworzy <a href="/encapsulation">encapsulation</a>, co oznacza, że dane są zawsze przetwarzane w kontekście lokalnym, który jest nieliczny, więc nie musimy myśleć o całej aplikacji naraz.
- Możemy podzielić aplikację na setki tysięcy małych części, które mogą być rozwijane równolegle przez różne osoby i po prostu zorganizować wspólny interfejs.
- Na aplikację możemy spojrzeć z perspektywy różnych warstw (abstrakcyjnie na całe moduły lub lokalnie na konkretne funkcje rozwiązujące określony algorytm).
- Podczas debugowania aplikacji (debugowania) mamy możliwość stopniowania aplikacji, pomijania lub zastępowania niektórych jej części.
- Możemy lepiej stosować **wzorce projektowe** i w ten sposób zapobiegać błędom i złożoności kodu, zanim one wystąpią.

Osobiście nie wyobrażam sobie, aby zespoły, w których pracuje więcej niż jedna osoba, mogły programować w inny sposób.

> **Uwaga:**
>
> Używanie obiektów obciąża komputer nieco większą ilością pamięci i procesora, dlatego też może to nieco obniżyć wydajność aplikacji. W rzeczywistym środowisku nie ma to jednak znaczenia, ponieważ przeprogramowanie aplikacji na obiekty powoduje pewne straty w wydajności (zwykle rzędu kilku procent), ale oszczędza czas programistów (zwykle dziesiątki do setek procent). Czas ludzki jest zawsze dużo droższy (i bardzo ograniczony) niż czas komputerowy.
>
> > Nie zapominajmy też, że OOP bardzo upraszcza całą aplikację i pozwala na ukończenie dużych aplikacji w rozsądnym czasie. Wiele złożonych aplikacji byłoby prawie niemożliwych do zaprogramowania bez obiektów.

Stosunek do świata rzeczywistego
-------------------------

Podstawowym celem OOP w projektowaniu oprogramowania jest jak najdokładniejsze symulowanie właściwości, zachowań i zasad świata rzeczywistego. Obiekty w OOP reprezentują rzeczywiste byty. Ten sposób myślenia pozwala nam budować ogromne, złożone systemy, które można dobrze zrozumieć, rozwiązywać problemy świata rzeczywistego w sposób wewnętrzny, tak jak byłyby one rozwiązywane bez komputera, a zasady działania można wyjaśnić prawdziwym ludziom.

Na przykład, jeśli wdrażamy aplikację do zarządzania treścią, sensowne jest umieszczenie całej wewnętrznej logiki w wielu rzeczywistych encjach (artykuł, autor, kategoria) i budowanie sesji nie według sztucznie wygenerowanych identyfikatorów rekordów (jak to się tradycyjnie robi w bazach danych), ale według rzeczywistych relacji.

Przykład konkretnej implementacji:

```php
class Article
{
    private Author $author;

    /** @var Kategoria[] */
    private array $categories;

    private string $title;
}

class Author
{
    private string $name;
}

class Category
{
    private string $name;
}
```

Jak widać, klasa `Article` nie tylko zawiera parametry techniczne, takie jak identyfikator rekordu z autorem, ale jest rzeczywistym powiązaniem z encją Author, która również ma swoje własne właściwości.

Wyjaśnienia dotyczące konkretnej implementacji i składni można znaleźć w <a href="/uvod-do-oop">tutorialu wprowadzającym</a>.
