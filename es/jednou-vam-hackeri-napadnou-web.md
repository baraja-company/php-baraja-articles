Un día los hackers atacarán su sitio web
========================================

> id: a11ca0a6-77e1-4ba4-8eca-83449c9cbf48
> slug:
> 	cs: jednou-vam-hackeri-napadnou-web
> 	es: un-dia-los-hackers-atacaran-su-sitio-web
> 
> publicationDate: '2020-08-04 12:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: a74c964823fb0c4231b711d113055a24

¿No te va a pasar eso? :DDDDDDD

Al gestionar más de 300 sitios web, de vez en cuando tengo varias urgencias. A veces son bastante acalorados, pero a menudo es una completa trivialidad. Si, como yo, has estado tentado por la programación en el pasado, y también sabes que [la programación es un dolor](http://borisovo.cz/programming-sucks-cz.html), seguramente estarás de acuerdo conmigo.

En las garras de los malvados hackers
----------------------

Cuando una aplicación web se hace popular, se convierte en un objetivo tentador para los hackers. Su motivación no suele ser hacer caer todo el sitio web, sino todo lo contrario. De hecho, los **hackers quieren que usted, como administrador del servidor, los desconozca por completo**. Un buen hacker espera durante meses, vigilando el servidor web y consiguiendo lo más valioso: sus datos.

Para proteger a sus usuarios, es imprescindible cifrar todos los datos y tener varias capas de protección. Para las contraseñas, utilice una de las funciones lentas como [Bcrypt + salt](https://php.baraja.cz/hashovani), Argon2, PBKDF2 o Scrypt. Michal Spacek ya ha escrito sobre [security rating](https://pulse.michalspacek.cz/passwords/storages/rating#slow-hashes), y le agradezco que se haya sumado al artículo. Como propietario de un sitio web, debe insistir en el hash de la contraseña adecuado cuando discuta con un programador. De lo contrario, vas a llorar, al igual que muchas personas antes que tú que también se engañaron a sí mismos que no les concernía.

¿Sabes cuándo te atacan?
---------------------------------

Me he dado cuenta de que cuando un servidor web alcanza un determinado umbral de tráfico (en la República Checa, es de unos 50+ visitantes al día), empieza a recibir una serie de ataques dirigidos a él de la nada. Para que no sea fácil, el atacante siempre tiene ventaja, porque puede elegir qué parte de la infraestructura empezar a atacar y tú -como propietario del servidor web- tienes que reconocerlo a tiempo, defenderte y estar mejor preparado para la próxima vez.

¿Cuántos ataques se han dirigido a usted en el último mes, por ejemplo? ¿Lo sabes exactamente? ¿A cuántos de ellos has defendido? ¿Alguien más tiene acceso a su servidor?

¿Y si alguien te ataca ahora mismo? Y ahora... ¿y ahora también...?

Desgraciadamente, no hay una forma única de reconocer un ataque. Pero hay herramientas que pueden darle una ventaja significativa.

Lo que mejor me ha funcionado últimamente:

- Cuando un atacante no sabe dónde están tus servidores físicamente**, tiene una posición mucho más difícil. La información de la arquitectura física se puede ofuscar muy bien con [Clouflare](https://www.cloudflare.com/), que proxy (y encripta bidireccionalmente) toda la comunicación de la red. También cuenta con otras ventajas, como una recuperación más rápida desde el extranjero, protección automática contra ataques DDOS, compresión de activos y, por último, el bloqueo automático de los visitantes no deseados. Utilizo Cloudflare para todos mis sitios web (y casi todos los proyectos utilizan tecnologías diferentes).
- **Registro de errores**. Siempre y todo. Lo más probable es que su aplicación contenga muchos errores cometidos por su programador. Ya sean [excepciones no capturadas] (https://php.baraja.cz/vyjimky), errores de aplicación, inyección SQL, etc. Si estás programando en PHP y no entiendes de registros, hay suficientes problemas que [Tracy](https://tracy.nette.org/) (y posiblemente otras herramientas como [Sentry](https://sentry.io/)) y los registros de acceso en el propio servidor detectarán. El registro de errores no es algo agradable, es una obligación. Registro los errores en todos los sitios que mantengo activamente, y hago que me envíen información sobre nuevos errores a mi correo electrónico para conocerlos inmediatamente.
- **Plataforma segura y actualizada**. ¿Tienes WordPress en tu sitio? ¿Sabes cómo actualizarlo correctamente? ¿Conoce todos los riesgos a los que se enfrenta? Cuando algo es gratis, casi siempre es a costa de otra cosa. Personalmente, utilizo la versión 3 de [Nette framework](https://nette.org/cs/) para el desarrollo de aplicaciones, que actualizo semanalmente y busco activamente vulnerabilidades de seguridad para parchear. Al igual que usted lleva su coche a revisión regularmente, necesita llevar su sitio web a revisión regularmente y al menos una vez al mes. El mantenimiento del sitio incluye la actualización de todas las bibliotecas externas y la supervisión constante de los registros. Si está utilizando una versión antigua de una biblioteca, es muy probable que un atacante conozca la vulnerabilidad e intente explotarla.
La cuestión es cuándo
Un gran número de personas en mi área subestiman la seguridad de una manera increíble y exponen a Internet contenidos que son muy fáciles de romper y explotar.

De hecho, incluso las grandes empresas cometen errores, incluso en cosas triviales como el ataque [XSS](https://www.zive.cz/clanky/vyvojar-objevil-zranitelnost-v-seznamu-dokazal-mezi-vysledky-propasovat-zakazany-kod/sc-3-a-200023/default.aspx) (cross-site scripting).

La cuestión no es si alguien va a romper su sitio web. La cuestión es cuándo ocurrirá y si estará lo suficientemente preparado para afrontarlo. Tal vez un hacker ha tenido acceso a su servidor durante meses y usted no lo sabe (todavía).

Qué hacer cuando hay problemas
-------------------------------

Cuando se descubre el trabajo del hacker, suele ser demasiado tarde y el hacker ha hecho todo lo que tenía que hacer. Es muy probable que haya colocado mallware por todo el servidor y que tenga al menos un control parcial de la máquina.

Este es un **problema de tipo caótico y hay que actuar rápido**.

Dado que esta es la última etapa del ataque, personalmente recomiendo desconectar el servidor web de Internet por completo y empezar a averiguar poco a poco lo que ha sucedido. Además, nunca sabremos con exactitud si el servidor está ocultando algo más. El atacante siempre tiene una ventaja significativa.

Si decidimos poner la última copia de seguridad que funciona en el servidor, estaremos ayudando mucho al atacante. Ya sabe cómo entrar en su servidor y no hay nada que le impida volver a hacerlo en unas horas. Además, es probable que la copia de seguridad contenga mallware o algún otro tipo de virus.

Aunque esta es una opción muy impopular entre mis clientes, personalmente siempre recomiendo **borrar completamente los sitios de WordPress** primero, o cualquier sistema que el cliente no actualice y que tenga muchas amenazas de seguridad. Por ejemplo, si aloja 30 sitios en un servidor con más de 2 años de antigüedad, tiene prácticamente garantizado que al menos uno de ellos contiene algún tipo de vulnerabilidad.

Si no entiende el problema, es mejor que se ponga en contacto con un especialista adecuado desde el principio que conozca a fondo su problema y entienda las cosas en un contexto amplio.

**Nota ética:**

Si usted gestiona un sitio web para un cliente que ha tenido un incidente de seguridad, es su responsabilidad informarle. Si se filtra algún dato (como las contraseñas, pero a menudo mucho más), también hay que informar a los usuarios finales de que su contraseña es de dominio público y deben cambiarla en cualquier lugar. Si utilizas una función de hash lenta (por ejemplo, **Bcrypt + sal**), harás que sea mucho más difícil para un atacante descifrar las contraseñas. (Desgraciadamente, [Las contraseñas de Bcrypt pueden ser descifradas](https://arstechnica.com/information-technology/2015/08/cracking-all-hacked-ashley-madison-passwords-could-take-a-lifetime/)). Si quieres recibir información periódica sobre si tu cuenta ha sido filtrada, te recomiendo que te registres en [¿He sido pwned?](https://haveibeenpwned.com/). Para obtener más información sobre [violación de seguridad](https://m.uoou.cz/vismo/zobraz_dok.asp?id_ktg=5020&n=poruseni-zabezpeceni), visite el sitio web de OOOO.

Hábitos atómicos
--------------

En los últimos seis meses terminé de leer el libro [Atomic Habits](https://www.melvil.cz/kniha-atomove-navyky/), que me abrió los ojos. Describe un proceso de mejora incremental del 1% cada día que hará mucho a largo plazo.

Dado que las empresas y los particulares se dirigen a mí en diversas etapas de la deuda tecnológica, me gustaría resumir las peores cosas:

- **FTP para la comunicación con el servidor no tiene sentido**. Es un protocolo inseguro que está controlado por contraseña. Si quieres dar acceso a alguien, tiene que conocer la contraseña y puede hacer lo mismo que tú. Si quieres quitarle el acceso, no puedes, la única forma es cambiar la contraseña. Es mejor cambiar completamente a SSH y utilizar claves RSA. A menudo me sorprende que incluso las empresas resuelvan este problema hoy en día. Nunca hagas una tabla con todas las contraseñas. Y, desde luego, ¡nunca compartida por todo el equipo!
- El **código fuente está en Git**, los datos en la base de datos y el contenido estático (imágenes, PDFs, ...) en archivos en disco. La elección de un almacenamiento de datos equivocado empeora el diseño y la seguridad de la aplicación. La URL de un PDF (por ejemplo, con una factura) debe contener un contenido generado aleatoriamente que sólo el destinatario conoce. En el caso de contenidos extremadamente sensibles (como un extracto bancario), es importante encriptar el contenido en el disco y proporcionarlo sólo después de iniciar la sesión.
- Las partes no públicas del sitio deben contener la cabecera META `noindex` y `nofollow` para evitar ser indexadas por Google. Los contenidos sensibles que sus competidores no deben conocer deben estar protegidos por una contraseña.
- Desactive el [listado de contenido del directorio](https://www.simplified.guide/apache/disable-directory-listing) en su servidor. Si se configura de forma inadecuada, todo el sitio puede ser rastreado por la máquina para encontrar archivos sensibles.
- ¡Proyecto raíz! Nunca debe haber una URL directa a los archivos de configuración. Por ejemplo, [Nette lo hace bien](https://doc.nette.org/cs/3.0/quickstart/getting-started#toc-obsah-web-projectu) desde el primer tutorial. Lo mismo ocurre con los [repositorios git disponibles públicamente](https://smitka.me/open-git/). Es muy fácil subestimar este tipo de vulnerabilidad y las consecuencias suelen ser trágicas.
- El fallo también puede surgir de un error de desarrollo involuntario. Es muy importante hacer una revisión del código por parte de un humano vivo para cada cambio. Muchos errores son descubiertos por [PHPStan](https://github.com/phpstan/phpstan) o herramientas similares antes de que un humano vea el código.
- Nunca [envíes las contraseñas en forma legible por correo electrónico](https://www.lupa.cz/clanky/reset-a-poslani-hesla-v-citelne-podobe-e-mailem-nebezpecna-praktika/), siempre hay otra forma de solucionarlo.
- Problemas de seguridad en versiones antiguas de bibliotecas y software. Los robots rastrean Internet cada segundo y atacan agujeros de seguridad conocidos (inyección SQL, XSS, CSRF, ...). Muchos de los agujeros conocidos tienen versiones antiguas de WordPress.

Hay muchas más vulnerabilidades y los problemas varían de un proyecto a otro. Si necesita hacer una auditoría rápida del servidor, le recomiendo que utilice [Maldet](https://www.rfxn.com/projects/linux-malware-detect/) y que luego contrate a un especialista adecuado para que le ayude a hacer una [auditoría completa del sitio](https://baraja.cz/audit-webu), y no sólo por razones de seguridad.

Los ingenieros de seguridad son como el plástico en el océano: son eternos. Acostúmbrate.

El principio de acción y reacción de la naturaleza
-------------------------------

También habrás notado que la naturaleza siempre utiliza el principio de reacción. Esto significa que algo ocurre en un determinado entorno y los organismos que lo rodean reaccionan a ello de diferentes maneras. Este enfoque tiene muchas ventajas, pero una enorme desventaja: siempre te quedarás atrás.

Como persona pensante (diseñador, desarrollador, consultor) tienes una gran ventaja, y es la de ser procesable, es decir, conocer de antemano una parte de los grandes riesgos y evitar activamente que se produzcan en primer lugar. Puede que nunca se eviten todas las meteduras de pata, pero al menos se pueden mitigar las consecuencias, o crear mecanismos de control que detecten los problemas con antelación para tener tiempo de reaccionar.

La mayoría de los ataques se producen durante un largo periodo de tiempo, y si se registra, normalmente tendrá tiempo suficiente para detectar el problema.
