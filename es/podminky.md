Condiciones y ramificaciones
============================

> id: '2cea5541-6879-4763-a518-cb21bf9021dd'
> slug:
> 	cs: podminky
> 	es: condiciones-y-ramificaciones
> 
> publicationDate: '2019-09-07 20:25:57'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: bc4140fc49413be3f2f2b2264b8ea5e6

> **Atención:** Este artículo fue escrito hace muchos años y parte de la información puede ser obsoleta o incorrecta. Téngalo en cuenta al leerlo.

Se acabaron los programas lineales. El principio más básico de cualquier programa es "lo que sucede cuando....". Una condición puede escribirse como una declaración lógica, que puede ser válida (la condición se satisface) o inválida (entonces no se ejecuta o se ejecuta su opuesto exacto). Ambos son fáciles de definir.

Notación general
------------

En general, una condición puede escribirse como una declaración lógica. La condición puede cumplirse o no. Es una buena idea contar con las dos opciones posibles. Si hay varias alternativas, se denomina **condición anidada**.

Ejemplo:

```php
if (hodnota   operace   hodnota) {
	// Se activa si se cumple la condición
} else {
	// Se activa si la condición no se aplica
}
```

No siempre tenemos que definir ambas opciones (a veces es completamente innecesario). De hecho, podemos definir la situación si sólo se cumple la condición. Esto se hace de la siguiente manera:

```php
if (hodnota   operace   hodnota) {
	// Se activa si se cumple la condición
}
```

Operadores lógicos
--------------------------

| Operador Significado
|----------|---------
| `==` | Igual a
| `===` | Es igual y tiene el mismo tipo de datos (*cualquier cosa puede ser comparada con cualquier cosa, pero la condición sólo se satisface si es un valor del mismo tipo de datos (por ejemplo, número, texto, ...)*)
| No es igual
| `<=` | Igual o mayor que
| `>=` | Igual o menor que
| Mayor
| Menos

Ejemplo real
--------------------------

```php
$a = 5;
$b = 3;
if ($a === $b) {
	// bloque que se imprime si $a es igual a $b
} else {
	// bloque que se imprime si $a NO es igual a $b
}
```

Condiciones anidadas
--------------------------

Desgraciadamente, la salida es sólo `true` (válido) y `false` (inválido). Por lo tanto, si queremos considerar múltiples posibilidades, tenemos que anidar múltiples condiciones unas dentro de otras. Esto se llama **condición anidada**. Está anidado porque una de las soluciones a la condición es simplemente otra condición.

```php
$a = 5;         // bolsillo izquierdo
$b = 3;         // bolsillo derecho
$kapsa = true;  // ¿Tengo un bolsillo?

if ($kapsa === true) {

	if ($a > $b) {
		echo 'En el bolsillo izquierdo hay más';
	} else {
		echo 'En el bolsillo derecho hay más';
	}

} else {
	echo 'No tienes un bolsillo';
}
```
