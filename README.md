# Introducción
El día 03/08/2020 y tras 2 años de trabajo, publiqué mi primer juego en Steam, [Cursed Gem](https://store.steampowered.com/app/1194480/Cursed_Gem/). Desgraciadamente el juego terminó publicado en páginas piratas de cracks/torrents a las 24h. Quiero recalcar que si mi juego ha sido pirateado, significa que es lo suficientemente atractivo como para que alguien se tome la molestia en piratearlo. Además hay varios artículos que hablan sobre como este fenómeno puede traducirse como una campaña de márketing y dar mayor visibilidad a pequeños desarrolladores indie como yo.

Este artículo no pretende debatir sobre si es bueno o no que pirateen tu software, o si es bueno o no el uso de DRM, o si es bueno o no permitir/denegar el acceso a tu juego pirata. Simplemente quiero compartir un método sencillo de implementar, que añade una pequeña capa de protección a tu juego para detectar si el jugador lo ha comprado o lo ha pirateado. En ese momento es decisión tuyo de implementar la acción que más desees, por ejemplo mostrar un amigable mensaje o directamente bloquear el juego. Empezamos?

# Algo que debes saber sobre los sistemas de protección
No existe ningún método 100% infalible que permita protejer tu juego/aplicación. Da igual lo complejo que sea el sistema, da igual la contidad de encriptación y ofuscación que utilices, da absolutamente igual. Cualquier sistema que implementes podrá ser crackeado/pirateado si caen en manos experimentadas y se le dedica el tiempo suficiente.

Los sistemas de protección de hoy en día se basan en complicar mucho la tarea de cracking, de tal forma que el crack aparezca varios días (o semanas!) después de la publicación del juego, consiguiendo que jugadores impacientes terminen comprando el juego en lugar de esperar el crack. De hecho es muy común que empresas grandes decidan actualizar y eliminar el DRM de sus juegos el día siguiente que haya sido crackeado. Estos sistemas de protección simplemente buscan ganar tiempo.

Dicho esto, no creo que sea necesario recordarlo, pero el sistema que he implementado en mi juego tampoco es infalible. Si un cracker le dedica tiempo, terminará pirateando el juego. La gracia de este sistema que voy a explicar es que el cracker no sabe que su "crack" no funciona, convirtiendo el juego pirateado en una versión "demo" del juego para que, con un poco de suerte, el jugador pirata decida comprarlo.

# Escenario, herramientas, víctima y verdugo.
## Escenario
Cursed Gem es un juego programado en [Godot Engine](https://godotengine.org/). No quiero entrar en detalles ya que el artículo se alargaría mucho, pero básicamente tenemos que saber que cuando exportamos un juego, lo que realmente estamos obteniendo es una copia del motor del juego (el engine, sin las herramientas de edición) y todo nuestro código fuente empaquetado en un pichero PCK. En otros motores de juego lo que normalmente obtenemos es una compilación, es decir, nuestro código fuente es linkado y compilado, generando un binario único que se puede ejecutar/jugar. Con Godot eso no ocurre ya que no existe compilación. GDScript es un lenguaje interpretado (como Python) y eso nos otorga muchas ventajas y debilidades.

Qué significa? Pues que es muy fácil revertir el proceso de empaquetado y obtener el código fuente a partir del fichero PCK.
