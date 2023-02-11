Ataque de piratas informáticos a la agencia
===========================================

> id: '7d5edd94-cce4-4a32-9ec5-51260dcd1653'
> slug:
> 	cs: senior-5-utok-hackeru-na-agenturu
> 	es: ataque-de-piratas-informaticos-a-la-agencia
> 
> publicationDate: '2023-02-11 14:20:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: '18511f7b3ff80124720244a93e140932'

Una historia de 2017: trabajas como desarrollador principal en una agencia y gestionas unos 300 proyectos de diversa envergadura que la empresa ha desarrollado en ese tiempo. La mayoría son sencillas aplicaciones Nette con hasta 10 plantillas, algunos formularios y tablas de bases de datos. Nada del otro mundo. No sabes mucho sobre los proyectos, porque cada uno fue desarrollado por un proveedor ligeramente diferente, la gente entra y sale de la empresa, y esperas conseguirlo de alguna manera. Como la empresa optimiza mucho los costes, aloja unos 50 proyectos a la vez en un servidor.

De repente, un jefe de proyecto viene corriendo a decirle que uno de los sitios importantes del cliente ha dejado de funcionar. En lugar del sitio real, aparece una pantalla negra y todo tipo de texto que dice que el sitio ha sido pirateado y tiene acceso a todo el servidor.

Una vez más, no sabes (ni puedes saber válidamente) tanto sobre la arquitectura y el contenido de los proyectos, y muchos proyectos se ejecutan sólo en el servidor. El proyeccionista te empuja a que el sitio debe funcionar y, sin embargo, desconoces el alcance del ataque y dónde ha ido a parar el atacante, o si hay puertas traseras del pasado.

¿Cómo se decide?

1. No haces nada e intentas analizar primero el incidente.
2. Desconectas el servidor. No se conoce el alcance de los daños y el atacante puede seguir penetrando en el servidor y robar datos. Al mismo tiempo, significa que muchos sitios no están disponibles.
3. Usted realiza una restauración de un sitio específico a partir de una copia de seguridad de hace un día, y mientras tanto se ocupa de lo sucedido. Pero el atacante puede recuperar el acceso y seguir robando datos.
4. Otra solución...
