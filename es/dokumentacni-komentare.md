Comentarios sobre el documental, ¿en checo o en inglés?
=======================================================

> id: e5f1bf13-ab9e-4a70-a95c-77fb50aa4878
> slug:
> 	cs: dokumentacni-komentare
> 	es: comentarios-sobre-el-documental-en-checo-o-en-ingles
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1'
> sourceContentHash: a33af5a14338e9895fe87087362bd476

Escribir la documentación lleva tiempo y a menudo nadie la lee después de ti, por lo que es una buena práctica escribir comentarios directamente en el código fuente. Sin embargo, una gran cantidad de texto desordena innecesariamente el código, que luego puede no caber en el monitor al mismo tiempo, reduciendo de nuevo su legibilidad.

Por lo tanto, cualquier código debe ser **autoexplicativo**, es decir, al leerlo, debe quedar inmediatamente claro lo que hace, y debe hacer una sola cosa.

Por ejemplo, si una función se llama `getUserProfileById($id)`, está muy claro lo que hace dicha función y qué valor de retorno podemos esperar. Por eso se suelen utilizar comentarios para describir sólo los parámetros de entrada y salida + una breve explicación textual del principio (si se trata de algo más complejo). Cuando el código está destinado a varias personas, es conveniente incluir el autor y la fecha de creación.

```php
/**
 * Devuelve TRUE si el valor pasado es una dirección IPv6 válida
 * Expresión regular alternativa:
 * /^(((?=(?>.*?(::))(?!.+\3)))\3?|([\dA-F]{1,4}(\3|:(?!$)|$)|\2))
   (?4){5}((?4){2}|((2[0-4]|1\d|[1-9])?\d|25[0-5])(\.(?7)){3})\z/i
 * @autor Jan Barasek
 */
function isIpV6(string $value): bool
{
   if (str_contains($value, ':')) {
      return (bool) filter_var($value, FILTER_VALIDATE_IP, FILTER_FLAG_IPV6);
   }

   return false;
}
```

Del comentario se desprende que la función espera una dirección IPv6 como entrada y devuelve un booleano `TRUE` o `FALSE` dependiendo de cómo vaya la validación.

La anotación bootstrap se utiliza para indicar valores mediante claves estandarizadas, que muchos editores pueden procesar posteriormente y, por ejemplo, indicar parámetros al llamar a la función, o pasar dependencias automáticamente.

Los comentarios directamente dentro del código se utilizan sólo en casos especiales. Si el código necesita un comentario interno, suele ser una indicación de que debe dividirse en varias partes más pequeñas que se ocuparán de su propia parte del programa.

Idiomas
--------------

Es habitual nombrar siempre las clases, métodos, funciones y variables en inglés (para que el código sea legible para un gran número de programadores), las claves están relativamente estandarizadas con una sintaxis uniforme, por lo que el idioma tampoco importa ahí, y para el texto de ayuda depende del uso del proyecto. Si se trata de un proyecto pequeño para un equipo estable de personas, el inglés no es necesariamente un problema, al contrario, al menos todos entienden la descripción perfectamente.

Motivación para utilizar la anotación
-------------------

Los caracteres especiales (signos de llamada) pueden ser notados por otras herramientas más allá del editor, por lo que es útil, por ejemplo, poner sus pruebas (en qué entradas espero qué salida) directamente encima de la definición de la función, de esta manera nunca se olvida el lugar donde se escriben las pruebas y al mismo tiempo todo programador tiene una idea inmediata de lo que hace la función.

He aquí un pequeño ejemplo de cómo podría ser una definición de prueba (es sólo un concepto de notación, no es una herramienta de prueba específica):

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
@test Geolokace I---> $ip => {1-255}.{1-255}.{1-255}.{1-255} O---> ($return['país'] == 'ES')

Pokusí se načíst článek s vysokým ID a při testování mrkne na modifikátory (filtry):
@test Neexistující článek I---> $id => {1000000+} O---> [HEADER:4**:3**|NOCONTENT]

Vrátí počet rohů jednorožce:
@test Jednorožec I---> $maRohy => TRUE O---> 1

Kolik prstů ukazuji?
@test Prsty I---> $ukazuji => {0-10|NAME:prsty} O---> ($return == {*|NAME:prsty})
```

Lo que generalmente se escribiría, por ejemplo, según la regla:

```php
@test <název_testu> I---> $<proměnná> => <hodnota> O---> [<modifikátor>:<hodnota>] (<výraz_platnosti>)
```

Dentro de las pruebas he utilizado un generador de valores aleatorios utilizando expresiones regulares.
He aquí algunos ejemplos:

```php
{<hodnota><směr_nerovnosti>}           {500+}  {250-}   {-200-(-50)}
{<začátek>-<konec>}                    {0-5}   {a-z}   {a-f0-9}
{<Hodnota1>|<Hodnota2>|<Hodnota3>}     {Praha|Kladno|Brno}
{<výraz>|<modifikátor>:<hodnota>}      {a-z0-9|len:6}  {*|len:6}
{*|<modifikátor>:<hodnota>}            {*|randomWord:5}
```
