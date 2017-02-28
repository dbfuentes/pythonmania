+++
title = "Migrando este blog fuera de Wordpress.com"
slug = "migrando-este-blog-fuera-de-wordpress-com"
description = "Migrando este blog fuera de Wordpress.com a un generador de contenidos estáticos"
date = "2016-08-06T23:06:07-04:00"
lastmod = "2016-08-10T20:36:01-04:00"
categories = ["general"]
tags = ["web"]
draft = false
author = "Daniel Fuentes"
lang = "es"
+++

Hace un tiempo que estaba pensando mover este blog fuera de wordpress.com y
ahora que compre un dominio para el (`pythonmania.net
<https://www.pythonmania.net/es>`_ ), voy a aprovechar de hacerlo.

Si se preguntan por que mudarme, estos son algunos motivos:
<!--more-->

1. El editor WYSIWYG: [1]_ Puede ser útil la mayoría de las veces, sobre todo
   con un par de clicks uno puede dar el formato que uno quiere al texto...
   siempre que no sean trozos de código (como los que publico en este blog),
   ya que hay que escribirlo en modo html y si pasas a llevar el botón para
   el modo WYSIWYG, se pierde el formato del código y terminas con cualquier
   cosa... Es más incluso las notas al pie de pagina (como las de esta
   entrada), son mucho más fáciles de hacer `en comparación a wordpress.com
   <https://en.support.wordpress.com/splitting-content/page-jumps/>`_

#. Quería una forma fácil de escribir sin tener que estar siempre en linea.
   Con un generador de sitios estático puedo escribir en algún formato como
   reStructuredText [2]_ (que es mucho mas simple que el editor de Wordpress
   para escribir código o documentación) o Markdown y simplemente guardarlo
   localmente antes de publicarlo (total es simple texto plano, que puedo
   editar hasta con el bloc de notas).

#. Si quiero usar mi propio dominio, a pasar por caja. Si quiero usar algunos
   temas, a pasar por caja. ¿modificar el CSS?, a pasar por caja. ¿Modificar
   el HTML? o ¿acceder a la base de datos del blog?, NO se puede (ni aunque
   pases por caja). No lo mal entiendan, no me importa pagar, pero hay cosas
   que aunque pagues igual no puedes hacer.

#. Si quiero crear un sitio/blog multilenguaje (por ejemplo para publicar
   ocasionalmente alguna traducción de alguna entrada), solo tengo `3
   opciones: <https://en.support.wordpress.com/set-up-a-multilingual-blog/>`_

   - En una misma entrada escribir en cada uno de los idiomas (horrible y
     confuso).

   - Crear una entrada por cada idioma (algo más decente, pero se crea una
     mezcla de idiomas rara en las categorías/tags).

   - o crear dos blogs, cada uno en un dominio (o subdominio) separado (esta si
     es más decente). Pero sea ninguna posibilidad de crear subsitios para cada
     lenguaje (ejemplo `pythonmania.net/es <https://www.pythonmania.net/es>`_
     y traducciones en `pythonmania.net/en <https://www.pythonmania.net/en>`_).

Algunas de estas cosas se pueden solucionar hosteando uno mismo una instalación
de wordpress.org, o usar algún otro CMS pero eso también trae problemas (y
notese que por el 2007 tuve una instalación de worpress hosteada por mi mismo):

1. Rendimientos regulares, por muy bueno que sea hosting.

#. Posibles agujeros de seguridad, ya sea en wordpress, en algún plug-in o en
   el propio php que genera las paginas.

#. Que la base de datos se corrompa o peor aun ser víctima de alguna `inyección
   SQL <https://es.wikipedia.org/wiki/Inyecci%C3%B3n_SQL>`_ o similar

#. Ataques al propio CMS (intentos de acceso al WP-admin por ejemplo) u oleadas
   de spam, aunque se pueden evitar con algunos trucos. [3]_

#. Y como olvidar que en cada actualización (cosa obligatorias para reducir
   los agujeros de seguridad) había que cruzar los dedos para que las no se
   rompiera la instalación de wordpress o algún plug-in.

Por eso decidí usar un generador de contenidos estáticos (aprovechando el
cambio), que en términos simples toma los archivos de texto de las entradas
(escritos en rst, Markdown, etc), le aplica una o mas plantillas y genera una
seria de archivos html con el contenido del sitio. Las ventajas de esto:

1. Necesidad de muy pocos recursos para hospedar un sitio web (básicamente
   casi cualquier servidor lo puede hacer).

#. Flexibilidad: trabaja con muchos formatos, tiene menos restricciones a
   la hora de escribir y organizar código.

#. Simplicidad en el mantenimiento posterior y mas resistente a fallas.
   Como son paginas generadas no es necesario estar actualizando el
   generador frecuentemente. Además no pasa y si falla el servidor, en el
   peor de los casos inicias otro servidor, vuelves a copiar los archivos
   al servidor y listo.

#. Mayor velocidad de carga de la página. (no tiene que generar la pagina
   cada vez que un usuario la visita como ocurre con php), además puedes
   usar un CDN para acelerar aun mas la carga de contenidos.

Claro que también hay algunas desventajas:

1. Hay que tener algunos conocimientos para su actualización (como mínimo
   saber un poco de Markdown o similares)

#. No hay contenido dinámico, salvo que uses servicios externos (mailchimp,
   disqus, etc.). Un ejemplo de esto son los comentarios, en donde hay tras
   opciones:

   - instalar un sistema de comentarios propio (por ejemplo instalar un
     sistema como isso, juvia, talkatv, etc.).

   - usar un servicio externo, como IntenseDebate, Livefyre, google-plus,
     o disqus (el que estoy probando en este blog).

   - o crear soluciones creativas `como esta
     <https://bernhard.scheirle.de/posts/2014/March/29/static-comments-via-email/>`_

Bueno aun me quedan cosas por hacer, para tener el sitio 100% funcional,
entre ellas:

- Escoger un sistema de comentarios, por el momento estoy probando disqus
  a ver como va.

- Comenzar a redirigir las paginas de pythonmania.wordpress.com a
  `pythonmania.net/es <https://www.pythonmania.net/es>`_, cosa que voy a
  hacer en los próximos días. **Actulización 2016-08-10:** ya se esta redirigiendo.

- Crear un nuevo tema, o personalizar este (me gusta la organización del
  tema, pero le falta algo de color, es un poco gris)

- Crear y configurar los feeds

Bueno, eso es por ahora.

.. [1]  Sigla en inglés que significa “lo que ves es lo que obtienes”. O sea
        es un editor que permiten escribir un documento viendo directamente
        el resultado final.

.. [2]  reStructuredText (a veces abreviato com RST, ReST, o reST) y se usa
        bastante en la documentación de python. Ver la `wikipedia.org
        ReST <https://en.wikipedia.org/wiki/ReStructuredText>`_

.. [3]  Un truco bastante efectivo que use en ese tiempo, era que uno de los
        campos obligatorios del formulario (como el nombre de quien comenta o
        el mismo texto del comentario) aparecía por defecto con un texto como
        “No-llenar” (y lo ocultaba con un poco de CSS). Obviamente creaba un
        campo extra (con otro nombre) en el formulario para ese dato. Los
        humanos como no ven el campo oculto (y en el remoto caso que lo vean,
        leían el mensaje “No-llenar”) lo dejaban igual sin modificar y
        llenaban el campo extra, en cambio los bots (como normalmente no
        procesan el CSS) modificaban el “No-llenar” por otra cosa, así que
        simplemente antes de guardar el comentario en la base de datos se
        comprobaba el campo del “No-llenar” y si este tenia otro texto, se
        descartaba el comentario.
