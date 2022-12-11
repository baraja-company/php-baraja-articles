Un día los hackers atacarán su sitio web
========================================

> id: a11ca0a6-77e1-4ba4-8eca-83449c9cbf48
> slug:
> 	cs: jednou-vam-hackeri-napadnou-web
> 	es: un-dia-los-hackers-atacaran-su-sitio-web
> 
> publicationDate: '2020-08-04 12:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '0f0ac4c091115e9c1f0833d318f34f5d'

¿Eso no te va a pasar a ti? :DDDDDDD

Al gestionar más de 300 sitios web, de vez en cuando tengo varias urgencias. A veces son bastante acalorados, pero a menudo se trata de una completa banalidad. Si, como yo, has estado tentado por la programación en el pasado, y también sabes que [programar es un coñazo](http://borisovo.cz/programming-sucks-cz.html), seguramente estarás de acuerdo conmigo.

En manos de hackers malvados
----------------------

Cuando una aplicación web se hace popular, se convierte en un objetivo tentador para los hackers. Su motivación no suele ser hacer caer todo el sitio web, sino todo lo contrario. De hecho, **los hackers quieren que usted, como administrador del servidor, los desconozca por completo**. Un buen hacker espera durante meses, vigilando el servidor web y consiguiendo lo más valioso: tus datos.

Para proteger a sus usuarios, es imprescindible cifrar todos los datos y disponer de varias capas de protección. Para las contraseñas, utilice una de las funciones lentas como [Bcrypt + salt](https://php.baraja.cz/hashovani), Argon2, PBKDF2 o Scrypt. Michal Spacek ya ha escrito sobre [índice de seguridad](https://pulse.michalspacek.cz/passwords/storages/rating#slow-hashes), y le agradezco que lo haya añadido al artículo. Como propietario de un sitio web, debe insistir en que el hash de la contraseña sea adecuado cuando hable con un programador. De lo contrario, te vas a poner a llorar, como muchas personas antes que tú que también se engañaron a sí mismas pensando que no les concernía.

¿Sabes cuándo te atacan?
---------------------------------

Me he dado cuenta de que cuando un servidor web alcanza un cierto umbral de tráfico (en la República Checa, es de unos 50 visitantes al día), empieza a recibir una serie de ataques dirigidos a él de la nada. Para que no sea fácil, el atacante siempre tiene ventaja, porque puede elegir qué parte de la infraestructura empezar a atacar y tú -como propietario del servidor web- tienes que reconocerlo a tiempo, defenderte y estar mejor preparado para la próxima vez.

¿Cuántos ataques le han dirigido en el último mes, por ejemplo? ¿Lo sabes exactamente? ¿A cuántos de ellos ha defendido? ¿Alguien más tiene acceso a su servidor?

¿Y si alguien te está atacando ahora mismo? Y ahora... ¿y ahora también...?

Por desgracia, no existe una forma única de reconocer un ataque. Pero hay herramientas que pueden darte una ventaja significativa.

Lo que mejor me ha funcionado últimamente:

- **Cuando un atacante no sabe dónde están físicamente tus servidores**, tiene una posición mucho más difícil. La información de la arquitectura física se puede ofuscar muy bien con [Cloudflare](https://www.cloudflare.com/), que proxyiza (y encripta bidireccionalmente) toda la comunicación de red. También tiene otras ventajas, como una recuperación más rápida desde el extranjero, protección automática contra ataques DDOS, compresión de activos y, por último, pero no menos importante, bloqueo automático de visitantes no autorizados. Utilizo Cloudflare para todos mis sitios web (y casi todos los proyectos utilizan tecnologías diferentes).
- **Registro de errores**. Siempre y en todo. Lo más probable es que su aplicación contenga muchos errores cometidos por su programador. Ya sean [uncaught exceptions](https://php.baraja.cz/vyjimky), errores de aplicación, inyección SQL, etc. Si estás programando en PHP y no entiendes de logging, hay bastantes problemas que [Tracy](https://tracy.nette.org/) (y posiblemente otras herramientas como [Sentry](https://sentry.io/)) y los logs de acceso en el propio servidor detectarán. El registro de errores no es algo agradable de tener, es una obligación. Registro los fallos de todos los sitios que mantengo de forma activa, y hago que me envíen información sobre nuevos fallos a mi correo electrónico para conocerlos de inmediato.
- Plataforma segura y actualizada**. ¿Tiene WordPress en su sitio web? ¿Sabe cómo actualizarlo correctamente? ¿Conoce todos los riesgos a los que se enfrenta? Cuando algo es gratis, casi siempre es a costa de otra cosa. Personalmente, utilizo [Nette framework](https://nette.org/cs/) versión 3 para el desarrollo de aplicaciones, que actualizo semanalmente y busco activamente vulnerabilidades de seguridad que parchear. Del mismo modo que lleva su coche al taller con regularidad, su sitio web debe ser revisado con regularidad y al menos una vez al mes. El mantenimiento del sitio incluye la actualización de todas las bibliotecas externas y la supervisión constante de los registros. Si utiliza una versión antigua de una biblioteca, es muy probable que un atacante conozca la vulnerabilidad e intente aprovecharse de ella.
La cuestión es cuándo
Un gran número de personas de mi zona subestiman la seguridad de una manera increíble y exponen a Internet contenidos que son muy fáciles de vulnerar y explotar.

De hecho, hasta las grandes empresas cometen errores, incluso en cosas triviales como el ataque [XSS](https://www.zive.cz/clanky/vyvojar-objevil-zranitelnost-v-seznamu-dokazal-mezi-vysledky-propasovat-zakazany-kod/sc-3-a-200023/default.aspx) (cross-site scripting).

La cuestión no es si alguien romperá alguna vez su sitio web. La cuestión es cuándo ocurrirá y si estará lo suficientemente preparado para afrontarlo. Puede que un hacker haya tenido acceso a su servidor durante meses y usted no lo sepa (todavía).

Qué hacer cuando surgen problemas
-------------------------------

Para cuando te enteras del trabajo del hacker, suele ser demasiado tarde y éste ya ha hecho todo lo que tenía que hacer. Es muy probable que haya colocado malware por todo el servidor y tenga al menos un control parcial sobre la máquina.

Se trata de un **problema de tipo caótico y hay que actuar rápido**.

Dado que esta es la última fase del ataque, personalmente recomiendo desconectar el servidor web de Internet por completo y empezar a averiguar poco a poco qué ha ocurrido. Además, nunca sabremos con exactitud si el servidor oculta algo más. El atacante siempre tiene una ventaja significativa.

Si decidimos volver a poner en el servidor la última copia de seguridad que funcionaba, estaremos ayudando mucho al atacante. Ya sabe cómo entrar en tu servidor y nada le impide volver a hacerlo dentro de unas horas. Además, es probable que la copia de seguridad contenga malware o algún otro tipo de virus.

Aunque esta es una opción muy impopular entre mis clientes, personalmente siempre recomiendo **borrar completamente los sitios de WordPress** primero, o cualquier sistema que el cliente no actualice y que tenga muchas amenazas de seguridad. Por ejemplo, si aloja 30 sitios en un servidor con más de 2 años de antigüedad, tiene prácticamente garantizado que al menos uno de ellos contiene algún tipo de vulnerabilidad.

Si no entiende el asunto, será mejor que se ponga en contacto desde el principio con un especialista adecuado que conozca a fondo su problema y entienda las cosas en un contexto amplio.

**Nota ética:**

Si gestiona un sitio web para un cliente que ha tenido un incidente de seguridad, es su responsabilidad informarle. Si se filtra algún dato (como contraseñas, pero a menudo mucho más), también debe informar a los usuarios finales de que su contraseña es de dominio público y deben cambiarla en cualquier lugar. Si utilizas una función hash lenta (por ejemplo, **Bcrypt + sal**), harás que a un atacante le resulte mucho más difícil descifrar las contraseñas. (Por desgracia, [Las contraseñas Bcrypt pueden ser descifradas](https://arstechnica.com/information-technology/2015/08/cracking-all-hacked-ashley-madison-passwords-could-take-a-lifetime/)). Si deseas recibir información periódica sobre si tu cuenta ha sido filtrada, te recomiendo que te registres en [¿He sido pwned?](https://haveibeenpwned.com/). Para obtener más información sobre [violación de seguridad](https://m.uoou.cz/vismo/zobraz_dok.asp?id_ktg=5020&n=poruseni-zabezpeceni), visite el sitio web de OOOO.

Hábitos atómicos
--------------

En los últimos seis meses terminé de leer el libro [Atomic Habits](https://www.melvil.cz/kniha-atomove-navyky/), que me abrió los ojos. Describe un proceso de mejora incremental del 1% cada día que hará mucho a largo plazo.

Como se me acercan empresas y particulares en distintas fases de endeudamiento tecnológico, me gustaría resumir lo peor:

- **FTP para la comunicación con el servidor no tiene sentido**. Se trata de un protocolo inseguro controlado por contraseña. Si quieres dar acceso a alguien, tiene que conocer la contraseña y puede hacer lo mismo que tú. Si quieres quitarle el acceso, no puedes, la única forma es cambiar la contraseña. Mejor cambiar completamente a SSH y utilizar claves RSA. A menudo me sorprende que incluso las empresas resuelvan este problema hoy en día. Nunca hagas una tabla con todas las contraseñas. Y, desde luego, ¡nunca una compartida por todo el equipo!
- El **código fuente está en Git**, los datos en la base de datos y el contenido estático (imágenes, PDF, ...) en archivos en disco. Elegir el almacenamiento de datos equivocado empeora el diseño y la seguridad de la aplicación. La URL de un PDF (por ejemplo, con una factura) debe contener un contenido generado aleatoriamente que sólo conozca el destinatario. Para contenidos extremadamente sensibles (como un extracto bancario), es importante cifrar el contenido en el disco y proporcionarlo sólo después de iniciar sesión.
- Las partes no públicas del sitio deben contener los encabezados META `noindex` y `nofollow` para evitar ser indexadas por Google. Los contenidos sensibles que sus competidores no deben conocer deben estar protegidos por contraseña.
- Desactive [listado de contenido de directorios](https://www.simplified.guide/apache/disable-directory-listing) en su servidor. Si se configura de forma inadecuada, todo el sitio puede ser rastreado para encontrar archivos confidenciales.
- ¡Proyecto raíz! Nunca debe haber una URL directa a los archivos de configuración. Por ejemplo, [Nette lo hace bien](https://doc.nette.org/cs/3.0/quickstart/getting-started#toc-obsah-web-projectu) desde el primer tutorial. Lo mismo ocurre con [repositorios git disponibles públicamente](https://smitka.me/open-git/). Es muy fácil subestimar este tipo de vulnerabilidad y las consecuencias suelen ser trágicas.
- El fallo también puede surgir de un error de desarrollo involuntario. Es muy importante que cada cambio sea revisado por una persona. Muchos errores son descubiertos por [PHPStan](https://github.com/phpstan/phpstan) o herramientas similares antes de que un humano vea el código.
- Nunca [envíes contraseñas legibles por correo electrónico](https://www.lupa.cz/clanky/reset-a-poslani-hesla-v-citelne-podobe-e-mailem-nebezpecna-praktika/), siempre hay otra forma de solucionarlo.
- Problemas de seguridad en versiones antiguas de bibliotecas y programas informáticos. Los robots rastrean Internet cada segundo y atacan agujeros de seguridad conocidos (inyección SQL, XSS, CSRF, ...). Muchos de los agujeros conocidos tienen versiones anteriores de WordPress.

Hay muchas más vulnerabilidades y los problemas varían de un proyecto a otro. Si necesita realizar una auditoría rápida del servidor, le recomiendo que utilice [Maldet](https://www.rfxn.com/projects/linux-malware-detect/) y que después contrate a un especialista adecuado para que le ayude a realizar una [auditoría completa del sitio](https://baraja.cz/audit-webu), y no sólo por motivos de seguridad.

Los ingenieros de seguridad son como el plástico en el océano: para siempre. Acostúmbrate.

El principio de acción y reacción de la naturaleza
-------------------------------

También se habrá dado cuenta de que la naturaleza utiliza casi siempre el principio reaccionario. Esto significa que algo ocurre en un entorno concreto y los organismos que lo rodean reaccionan a ello de diferentes maneras. Este enfoque tiene muchas ventajas, pero una gran desventaja: siempre te quedarás atrás.

Como persona pensante (diseñador, desarrollador, consultor) tienes una gran ventaja, y es ser procesable, es decir, conocer de antemano una cierta parte de los grandes riesgos y evitar activamente que ocurran en primer lugar. Puede que nunca evite todas las meteduras de pata, pero al menos puede mitigar sus consecuencias o crear mecanismos de control que detecten los problemas con antelación para tener tiempo de reaccionar.

La mayoría de los ataques se producen durante un largo periodo de tiempo, y si se registra, normalmente tendrá tiempo suficiente para detectar el problema.
