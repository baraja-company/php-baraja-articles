Uso inadecuado del Recolector de Basura
=======================================

> id: '842e30ab-1af1-4152-ba0f-3004ca3b620f'
> slug:
> 	cs: senior-8-garbage-collector
> 	es: uso-inadecuado-del-recolector-de-basura
> 
> publicationDate: '2023-02-11 14:35:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: ae5993313436e0d7c5291b6337b988ef

Usted es desarrollador de una gran aplicación heredada, en la que está introduciendo gradualmente PHPStan. Empiezas con el nivel 0, que es todo un reto, pero al final lo consigues. Pasas a los siguientes niveles, donde una parte de tu código empieza a informar de una variable $lock sin usar que deberías eliminar.

El código es el siguiente:

```php
public function processOrder(int $orderId): void
{
	$lock = Lock::createLock('pedido-' . $orderId);

	// Hay algo de lógica aquí...
}
```

Te dices a ti mismo que debe haber un bloqueo almacenado en la variable que alguien se olvidó de liberar más tarde, o tal vez está sucediendo dentro de otros métodos que se llaman más tarde. Así que decides eliminar la variable no utilizada, manteniendo sólo la llamada al método estático que crea el bloqueo.

¿Podría esta decisión provocar un error crítico?

En caso afirmativo, ¿por qué y cómo podría haber funcionado el mecanismo original?

Si no es así, ¿por qué no, y cómo sabe que siempre es una operación segura?
