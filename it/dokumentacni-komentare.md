Commenti documentari, ceco o inglese?
=====================================

> id: e5f1bf13-ab9e-4a70-a95c-77fb50aa4878
> slug:
> 	cs: dokumentacni-komentare
> 	it: commenti-documentari-ceco-o-inglese
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1'
> sourceContentHash: a33af5a14338e9895fe87087362bd476

Scrivere la documentazione richiede tempo e spesso nessuno la legge dopo di voi, quindi è una buona pratica scrivere commenti direttamente nel codice sorgente. Tuttavia, un sacco di testo ingombra inutilmente il codice, che poi potrebbe non adattarsi al monitor allo stesso tempo, riducendo di nuovo la sua leggibilità.

Pertanto, qualsiasi codice dovrebbe essere **autoesplicativo**, cioè quando lo si legge, dovrebbe essere immediatamente chiaro cosa fa, e dovrebbe fare solo una cosa.

Per esempio, se una funzione si chiama `getUserProfileById($id)`, è chiarissimo cosa fa tale funzione e quale valore di ritorno possiamo aspettarci. Ecco perché i commenti sono spesso utilizzati per descrivere solo i parametri di input e di output + una breve spiegazione testuale del principio (se c'è qualcosa di più complesso). Quando il codice è destinato a più persone, è educato includere l'autore e la data di creazione.

```php
/**
 * Restituisce TRUE se il valore passato è un indirizzo IPv6 valido
 * Espressione regolare alternativa:
 * /^(((?=(?>.*?(::))(?!.+\3)))\3?|([\dA-F]{1,4}(\3|:(?!$)|$)|\2))
   (?4){5}((?4){2}|((2[0-4]|1\d|[1-9])?\d|25[0-5])(\.(?7)){3})\z/i
 * @autore Jan Barasek
 */
function isIpV6(string $value): bool
{
   if (str_contains($value, ':')) {
      return (bool) filter_var($value, FILTER_VALIDATE_IP, FILTER_FLAG_IPV6);
   }

   return false;
}
```

È immediatamente chiaro dal commento che la funzione si aspetta un indirizzo IPv6 come input e restituisce un booleano `TRUE` o `FALSE` a seconda di come va la validazione.

L'annotazione bootstrap è usata per indicare valori usando chiavi standardizzate, che molti editor possono elaborare ulteriormente e, per esempio, suggerire parametri quando si chiama la funzione o passare dipendenze automaticamente.

I commenti direttamente all'interno del codice sono usati solo in casi speciali. Se il codice ha bisogno di un commento interno, questo è di solito un'indicazione che dovrebbe essere suddiviso in più parti più piccole che indirizzeranno la loro parte di programma.

Lingue
--------------

È consuetudine nominare sempre classi, metodi, funzioni e variabili in inglese (per rendere il codice leggibile per un gran numero di programmatori), le chiavi sono relativamente standardizzate con una sintassi uniforme, quindi la lingua non importa neanche lì, e per il testo di aiuto dipende dall'uso del progetto. Se si tratta di un piccolo progetto per un team stabile di persone, l'inglese non è necessariamente un problema, anzi, almeno tutti capiscono perfettamente la descrizione.

Motivazione per l'uso dell'annotazione
-------------------

I caratteri speciali (call sign) possono essere notati da altri strumenti oltre all'editor, quindi è utile, per esempio, mettere i suoi test (su quali input mi aspetto quale output) direttamente sopra la definizione della funzione, così non si dimentica mai il posto dove sono scritti i test e allo stesso tempo ogni programmatore ha un'idea immediata di cosa fa la funzione.

Ecco un piccolo esempio di come potrebbe essere una definizione di test (è solo un concetto di notazione, non è uno strumento di test specifico):

```php
Dosadí hodnoty a vynutí jejich dump do prohlížeče / konzole:
@test Název I---> $limit => {1-{5-8}}, $city => {Praha|Kladno|Brno|Plzeň} O---> [DUMP]

Vygeneruje náhodný hash o délce 6 znaků a ověří, zda je hlavička odpovědi jakákoli, kromě 201:
@test Název I---> $hash => {a-z0-9|len:6} O---> [HEADER:***|!HEADER:201]

Vygeneruje dvojici čísel a na výstupu ověří jejich součet:
@test Sčítání čísel I---> $a => {1-5}, $b => {7-12} O---> ($return == $a+$b)

Načte náhodný článek s ID mezi 1 až 30, ověří jeho délku nebo neprázdnost:
@test Získání textu článku I---> $id => {1-30} O---> (strlen($return) > 64 || $return != NULL)

Vygeneruje náhodnou IP adresu a ověří, zda je z České republiky:
@test Geolokace I---> $ip => {1-255}.{1-255}.{1-255}.{1-255} O---> ($return['paese'] == 'IT')

Pokusí se načíst článek s vysokým ID a při testování mrkne na modifikátory (filtry):
@test Neexistující článek I---> $id => {1000000+} O---> [HEADER:4**:3**|NOCONTENT]

Vrátí počet rohů jednorožce:
@test Jednorožec I---> $maRohy => TRUE O---> 1

Kolik prstů ukazuji?
@test Prsty I---> $ukazuji => {0-10|NAME:prsty} O---> ($return == {*|NAME:prsty})
```

Che sarebbe generalmente scritto, per esempio, secondo la regola:

```php
@test <název_testu> I---> $<proměnná> => <hodnota> O---> [<modifikátor>:<hodnota>] (<výraz_platnosti>)
```

All'interno dei test ho usato un generatore di valori casuali usando espressioni regolari.
Ecco alcuni esempi:

```php
{<hodnota><směr_nerovnosti>}           {500+}  {250-}   {-200-(-50)}
{<začátek>-<konec>}                    {0-5}   {a-z}   {a-f0-9}
{<Hodnota1>|<Hodnota2>|<Hodnota3>}     {Praha|Kladno|Brno}
{<výraz>|<modifikátor>:<hodnota>}      {a-z0-9|len:6}  {*|len:6}
{*|<modifikátor>:<hodnota>}            {*|randomWord:5}
```
