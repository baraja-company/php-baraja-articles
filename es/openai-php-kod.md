¿Qué hacer cuando la inteligencia artificial quite el trabajo a los programadores el año que viene?
===================================================================================================

> id: '0eccb5c1-9ab4-42bf-b27e-13917d616b7f'
> slug:
> 	- null
> 	es: que-hacer-cuando-la-inteligencia-artificial-quite-el-trabajo-a-los-programadores-el-ano-que-viene
> 
> cs: openai-php-kod
> publicationDate: '2022-12-23 13:30:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '573e4173dd9e7a2e7068c404fecf9b72'

Las posibilidades del modelo lingüístico de OpeanAI son increíbles. Basta con introducir el código fuente, solicitar su análisis, corrección y reescritura en otro lenguaje, y todo sucede automáticamente.

Cuando lo probé por primera vez, me sorprendió lo que se puede conseguir y lo potentes que son las herramientas a disposición del público en general.

Veamos un ejemplo:

**Pregunta:** Encuentre la vulnerabilidad de seguridad en esta función, y proporcione el código parcheado:

```php
function getThisMonthName(): string
{
	$czMonths = [
		'',
		'Enero',
		'Febrero',
		'Marzo',
		'Abril',
		'Mayo',
		'Junio',
		'Julio',
		'Agosto',
		'Septiembre',
		'Octubre',
		'Noviembre',
		'Diciembre',
	];

	return $czMonths[(int) date('m')];
}
```

**Respuesta:**

Una de las vulnerabilidades de seguridad de esta función es el peligro de explotar un ataque de inyección SQL. Cuando una entrada mal formateada o maliciosa se pasa a la función, la base de datos puede verse comprometida o explotada.

Una solución es utilizar constantes de mes predefinidas en lugar de la entrada obtenida de una llamada a la función date(). También podemos utilizar la función filter_var() con un filtro adecuado para asegurarnos de que la entrada es numérica y no contiene código malicioso.

Aquí está el código corregido:

```php
function getThisMonthName(): string
{
	$czMonths = [
		'',
		'Enero',
		'Febrero',
		'Marzo',
		'Abril',
		'Mayo',
		'Junio',
		'Julio',
		'Agosto',
		'Septiembre',
		'Octubre',
		'Noviembre',
		'Diciembre',
	];
	$month = filter_var((int) date('m'), FILTER_VALIDATE_INT);

	if ($month === false || $month < 1 || $month > 12) {
		return "";
	}

	return $czMonths[$month];
}
```

**Pregunta:** Convierte la implementación de la función a typescript.

**Respuesta:**

Aquí está la implementación reescrita de la función en TypeScript:

```js
function getThisMonthName(): string {
	const czMonths = [
	'',
	'Leden',
	'Únor',
	'Březen',
	'Duben',
	'Květen',
	'Červen',
	'Červenec',
	'Srpen',
	'Září',
	'Říjen',
	'Listopad',
	'Prosinec',
	];

	const month = filter_var((int) date('m'), FILTER_VALIDATE_INT);

	if (month === false || month < 1 || month > 12) {
		return "";
	}

	return czMonths[month];
}
```

**Solicitud:** ¡Esto no es un typescript válido!

**Respuesta:**

Pido disculpas por el error. Aquí está la versión corregida de la función en TypeScript:

```js
function getThisMonthName(): string {
	const czMonths = [
	'',
	'Leden',
	'Únor',
	'Březen',
	'Duben',
	'Květen',
	'Červen',
	'Červenec',
	'Srpen',
	'Září',
	'Říjen',
	'Listopad',
	'Prosinec',
	];

	const month = Number(new Date().getMonth()) + 1;

	if (month < 1 || month > 12) {
		return "";
	}

	return czMonths[month];
}
```

Nota: No existe la función filter_var() en TypeScript, así que la reemplacé llamando a la función getMonth() de la clase Date y luego convirtiéndola a número usando la función Number(). Esta versión de la función debería ser válida en TypeScript.

**Pregunta:** ¿Su implementación de Typescript contiene una vulnerabilidad de seguridad?

**Respuesta:**

Mi implementación corregida de la función en TypeScript no debería contener la vulnerabilidad de seguridad del exploit de ataque de inyección SQL que estaba presente originalmente en la implementación original de PHP. La función getMonth() de la clase Date devuelve un valor numérico del mes (0 para enero, 1 para febrero, etc.), por lo que no necesitamos pasar entrada desde la función date() y podemos evitar el peligro del exploit de ataque de inyección SQL.

Sin embargo, cualquier aplicación puede contener vulnerabilidades de seguridad, por lo que es importante examinar periódicamente el código en busca de posibles problemas de seguridad. Si sospecha que existe una vulnerabilidad de seguridad en su código, puede ponerse en contacto con expertos en seguridad o utilizar herramientas automatizadas de exploración de vulnerabilidades de seguridad.
