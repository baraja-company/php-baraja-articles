Wyrażenia regularne w PHP
=========================

> id: '32142161-6ace-4cfd-b472-b48031219e9b'
> slug:
> 	cs: regex
> 	pl: wyrazenia-regularne-w-php
> 
> perex:
> 	- 'Regulární výrazy a jejich kompletní vysvětlení. Hledání podřetězce, pokročilé nahrazování a generování řetězců.'
> 	- 'Wyrażenia regularne i ich pełne wyjaśnienie. Wyszukiwanie podciągów, zaawansowane podstawianie i generowanie ciągów znaków.'
> 
> publicationDate: '2020-03-08 13:37:38'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '392c8f14e6943dd8f345a9a134e0de92'

Wyrażenia regularne to narzędzia, które umożliwiają łatwe wyszukiwanie, sprawdzanie poprawności, porównywanie, dzielenie, zwijanie i zastępowanie ciągów znaków zgodnie z maską (wzorcem). Jest to bardzo wydajne i eleganckie narzędzie do zaawansowanej manipulacji ciągami znaków.

Maska
-----

Na początku musimy najpierw wymyślić wyrażenie regularne, które będziemy wykonywać. Jest on wprowadzany jako ciąg tekstowy, który ma wiele reguł i opcji konfiguracyjnych (jest to bardzo złożona technika).

Na początek warto zauważyć, że wyrażenie regularne jest obliczane sekwencyjnie od lewej do prawej strony, a jeśli istnieje wiele sposobów interpretacji ciągu znaków, zawsze używane jest największe możliwe dopasowanie (wyrażenie zachowuje się w sposób **głodny**, starając się przetworzyć jak najwięcej znaków).

Na zachowanie i strategię przetwarzania wyrażenia regularnego można wpływać za pomocą przełączników, których jest wiele.

Prosta weryfikacja, czy podany ciąg jest poprawną wiadomością e-mail
-----------------------------------------------

Jak w prosty sposób sprawdzić, czy ciąg `jan@barasek.com` jest poprawnym adresem e-mail bez konieczności dzielenia go na skomplikowane części lub przeglądania znak po znaku?

Odpowiedzi udzielają wyrażenia regularne (powyższe wyrażenie jest bardzo uproszczone na potrzeby przykładu, a rzeczywista implementacja walidacji adresu e-mail powinna być nieco bardziej skomplikowana):

```php
$mail = 'jan@barasek.com';
$regex = '/^.+@.+.(en|en|com)$/.';

if (preg_match($regex, $mail)) {
   echo 'E-mail jest ważny';
} else {
   echo 'E-mail nie jest ważny';
}
```

Przyjrzyjmy się wyrażeniu `/^.+@.+.(en|en|com)$/` nieco bardziej szczegółowo:

Najpierw musimy zawinąć całe wyrażenie w parę znaków `/` (na początku i na końcu), które mówią, gdzie wyrażenie się zaczyna, a gdzie kończy. Po znaku `/` na końcu wyrażenia następują ewentualne modyfikatory (ustawienia trybu przetwarzania wyrażeń).

Podczas przetwarzania wyrażenia postępuje się od lewej strony, znak po znaku. Każdy z nich ma swoje znaczenie, które przedstawiono w poniższej tabeli:

| Znak | Znaczenie | Opis | Przykład |
|------|-----------------|-------|-------------
| `^` | Początek łańcucha | Wymusza, że łańcuch musi zaczynać się w tym miejscu. | Wymusza, że łańcuch musi zaczynać się od sekwencji `+420` (przydatne np. przy sprawdzaniu poprawności liczb): `/^+420/`. |
| `$` | Koniec łańcucha lub linii | Wymusza, aby łańcuch lub linia kończyły się w tym miejscu. Koniec linii jest następnie potwierdzany przez `z`. <a href="https://phpfashion.com/vite-co-znamena-v-regularnim-vyrazu">Szczegółowe wyjaśnienie</a>. | Nazwa pliku musi być plikiem tekstowym (zakończonym kropką, a następnie ciągiem "txt"): `/ml.txt$/`. |
| `.` | Dowolny znak | Wyłapuje dokładnie dowolny znak. | Sprawdza, czy łańcuch zawiera dokładnie jeden z dowolnych znaków: `/^.$/`. |
| | Liczba | Wykrywa znaki `0-9` | Wykrywa numer telefonu, który nie zawiera spacji i ma 9 cyfr: `/^(\+420)?\{9}$/`. |
| Zezwala na stosowanie spacji między cyframi w numerze telefonu w postaci potrójnych cyfr: `/^(^d{3}}?){3}$/`.
| `+` | Wiele znaków, ale co najmniej jeden | Powtarza poprzednie podwyrażenie i próbuje wychwycić jak najwięcej znaków. Podwyrażenie musi być powtórzone co najmniej raz. | Wyłapuje jak najwięcej cyfr, ale co najmniej jedną: `/\d+/`. |
| `*` | Wiele znaków, może być żaden | Działa tak samo jak `+`, ale pozwala na złapanie pustego łańcucha (wartość nie musi być obecna). | Łapie tyle cyfr, ile jest możliwe, jeśli nie ma żadnej, łapie pusty łańcuch: `/`d*/`. |
| Nawiasy | Wskazuje podwyrażenie. Można tego użyć, aby zawrzeć kilka różnych znaczników, a następnie zażądać, na przykład, powtórzenia ich lub przechwycić ich zawartość w zmiennej. | Podzielmy kod pocztowy na dwie części zgodnie ze spacją, która jest opcjonalna i może być ich więcej niż jedna: `/^(\d{3})\s*(\d{2})$/`.
| | Zawiera podwyrażenie lub inne podwyrażenie. | Liczba zaczynająca się od `+420` lub `+421`: `/^+(420|421)^s*$/`. |
| Jeśli chcemy przechwycić w wyrażeniu znak, który w przeciwnym razie miałby specjalne znaczenie, musimy go wymazać w taki sam sposób, jak na przykład ciągi znaków w PHP. | Przechwytuje parę liczb oddzielonych kropką (gdybyśmy nie wymazali kropki, zostałaby ona zrozumiana jako "dowolny znak"): `/\d+/`.

Dla uzupełnienia podam pełną postać reguły sprawdzania poprawności wiadomości e-mail zaimplementowanej przez Nette:

```php
/**
 * Wyszukuje, czy ciąg znaków jest prawidłowym adresem e-mail.
 */
public static function isEmail(string $value): bool
{
   $atom = "[-a-z0-9!#$%&'*+/=?^_`{|}~]"; // RFC 5322 niecytowane znaki w części lokalnej
   $alpha = "a-zx80-xFF"; // superset IDN
   return (bool) preg_match("(^
      (\"([ !#-[\\]-~]*|\\\\[ -~])+\"|$atom+(\\.$atom+)*)  # quoted or unquoted
      @
      ([0-9$alpha]([-0-9$alpha]{0,61}[0-9$alpha])?\\.)+    # domain - RFC 1034
      [$alpha]([-0-9$alpha]{0,17}[$alpha])?                # top domain
   \\z)ix", $value);
}
```

`preg_match()` - walidacja według wzorca
------------------------------------

Podstawową funkcją do sprawdzania poprawności formatu i parsowania jest `preg_match()`, ma ona dwa parametry obowiązkowe, a trzeci może być użyty do określenia pola wyjściowego.

Przykład:

```php
$psc = '272 01'; // Kladno

if (preg_match('/^(\d{3})\s*(\d{2})$/', $psc, $parser)) {
   echo 'Kod pocztowy jest ważny [' . $parser[1] . ','. $parser[2] . ']';
} else {
   echo 'Kod pocztowy jest nieprawidłowy';
}
```

Kod zwróci: `Kod jest prawidłowy [272, 01]`.

Zwróć uwagę na pojedyncze nawiasy, których użyliśmy do podzielenia wyrażenia na kilka mniejszych części. Dzięki temu można uzyskać poszczególne podwyrażenia jako wpisy w tablicy. Następnie cała funkcja zwraca `true` lub `false` w zależności od tego, czy łańcuch został pomyślnie przechwycony.

Czasami jednak poruszanie się w kolejności numerycznej nawiasów jest bardzo trudne, ponieważ liczba może się zmienić lub może być ich po prostu za dużo. W takim przypadku można nadawać nazwy poszczególnym nawiasom, a następnie uzyskiwać dostęp do klawiszy za pomocą ich nazw.

Na przykład:

```php
$phone = '777 123 456';

preg_match('/^(?<operator>*(?<numer>[0-9 ]+)$/.', $phone, $parser);

echo $parser['operator']; // zwrócono 777
```

`preg_replace()` - zamiana według wzorca
----------------------------------------

Możliwe jest także zastępowanie ciągów znaków za pomocą polecenia regex, co jest szczególnie przydatne w przypadku różnych poprawek formatu po użyciu.

Załóżmy, że chcemy przechowywać numer telefonu wprowadzony przez użytkownika w liczbie całkowitej, ponieważ jest on wymagany przez bibliotekę innej firmy, ale użytkownicy mogą wprowadzać go w dość dzikich formatach.

W takim przypadku trzymam się dyktatu:

> "Bądź hojny w tym, co otrzymujesz, i surowy w tym, co wysyłasz".

Dlatego automatycznie dostosowujemy format. Najpierw używamy parsowania, aby rozbić łańcuch na poszczególne części, a następnie składamy go z powrotem zgodnie z numerami nawiasów:

```php
function formatPhoneNumber(string $phoneNumber): int
{
   return (int) preg_replace(
      '/^(\+\d{3})\s*(\d{3})\s*(\d{3})\s*(\d{3})$/',
      '$2$3$4',
      $phoneNumber
   );
}

echo formatPhoneNumber('+420 777 123 456');
```

Konstruowanie łańcucha znaków według wyrażenia regularnego
----------------------------------------

Regeksy mają również duże znaczenie w przypadku generowania nowych ciągów znaków według złożonego wzorca.

W czystym PHP nie ma takiej obsługi, ale możemy pobrać zewnętrzną bibliotekę <a href="https://github.com/icomefromthenet/ReverseRegex">ReverseRegex</a>, która to potrafi.

Na przykład, możemy wygenerować zestaw haseł na podstawie regexu `[a-z]{10}` i nic nas nie powstrzyma:

```txt
jmceohykoa
aclohnotga
jqegzuklcv
ixdbpbgpkl
kcyrxqqfyw
jcxsjrtrqb
kvaczmawlz
itwrowxfxh
auinmymonl
dujyzuhoag
vaygybwkfm
```

Zastosowanie jest następujące:

```php
use ReverseRegex\Lexer;
use ReverseRegex\Random\SimpleRandom;
use ReverseRegex\Parser;
use ReverseRegex\Generator\Scope;

require 'vendor/autoload.php';

$lexer = new  Lexer('[a-z]{10}');
$gen   = new SimpleRandom(10007);
$result = '';

$parser = new Parser($lexer, new Scope(), new Scope());
$parser->parse()->getResult()->generate($result, $gen);

echo $result;
```

W ten sposób generuję na przykład moje przykłady matematyczne w programie Nette w programie Presenter i jest to bardzo łatwe:

```php
public function actionRegex(): void
{
   $lexer = new Lexer('');
   $parser = new Parser($lexer, new Scope(), new Scope());
   for ($i = 0; $i <= 10; $i++) {
      $result = '';
      $gen = new SimpleRandom($i);
      $parser->parse()->getResult()->generate($result, $gen);
      dump($result);
   }
   $this->terminate();
}
```

Dla biblioteki ważne jest to, że dla tych samych danych wejściowych generuje ona wciąż to samo wyjście (nawet jeśli może się wydawać, że istnieje wiele możliwych łańcuchów do dopasowania dla każdego wyrażenia regularnego). Jeśli chcemy losowo zmieniać generowane wyrażenie, musimy również zmienić `nasiono`, na podstawie którego generowany jest łańcuch wyjściowy. Można do tego celu wykorzystać albo przedział nasion, albo funkcję `rand(1, 1e6)`.

Wyłapywanie i przetwarzanie błędów
-----------------------------

W PHP wyłapywanie błędów w regexach jest dość trudne, ale jest na to rozwiązanie.

Jest to szczegółowo wyjaśnione w artykule <a href="https://phpfashion.com/zradne-regularni-vyrazy-v-php">Treachable regular expressions in PHP</a> Davida Grudela.
