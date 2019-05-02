+++
title = "Paquetes recomendados para Sublime Text"
slug = "paquetes-recomendados-para-sublime-text"
date = "2019-05-01T23:15:56-03:00"
categories = ["general", "herramientas"]
author = "Daniel Fuentes"
draft = false
lang = "es"
+++

Hace algún tiempo escribí una lista de `paquetes recomendados para atom,
<https://www.pythonmania.net/en/2017/02/27/paquetes-recomendados-para-atom/>`_
ahora voy a hacer lo mismo para `Sublime Text 3 (ST3)
<http://www.sublimetext.com/>`_.

Sublime Text 3 (ST3) es un editor de texto liviano, multiplataforma y de
software propietario, que es muy similar a atom (soporta plugins, normalmente
hechos por la comunidad) y que es conocido por su facilidad de uso, que cuenta
con un gran apoyo de su comunidad [1]_ y que es es bastante rápido (mucho mejor
que atom al abrir archivos, cerrarlos, hacer búsqueda en ellos, etc., es muy
fluido y con mejor rendimiento que atom, debido a que esta escrito en C ++ y
Python para los plugins).

**Información General**

1. URL: `www.sublimetext.com <http://www.sublimetext.com/>`_
2. Desarrolladores: Jon Skinner (ex ingeniero de Google) y Will Bond
3. Plataformas: mac OSX (10.7 o superior), Windows y GNU/Linux
4. Licencia: software propietario

- tiene una prueba gratuita ilimitada, con un pop-up recordándote que puedes comprarlo (como lo hace winrar).
- la licencia cuesta $80 USD en el momento es que escribí esta entrada (es solo 1 pago por versión mayor).

En general es un editor increíble recién instalado, pero su mejor y más
poderosa característica viene de la posibilidad de mejor sus funciones usando
paquetes y al crear configuraciones personalizadas.

En esta entrada he escogido algunos paquetes más útiles y/o "interesantes"
que he encontrado para ST3.
<!--more-->

Características incluidas
=========================

Sublime Text 3 tienes algunas características incluidas por defecto (que en
otros editores hay que añadirlas vía plugins), entre las cuales las más
importantes son:

1. **Minimapa:** muestra una vista previa condensada tu código para una
   navegación rapida.
#. **Pestañas (ventanas) divididas:** te permite organizar editor en varias
   pestañas (ventanas) que esten una al lado de otra. Esto es util cuando estas
   haciendo pruebas cuando desarrollas (por ejemplo código de Python en una y
   al lado en paralelo un script de prueba) o cuando estas trabajando en
   Front-end (HTML a un costado y CSS/JavaScript al lado).
#. **Vintage Mode:** permite el uso de comandos de vi/vim en ST3.
#. Carga automática de la última sesión, que vuelve a abrir todos los archivos
   y carpetas que tenía abiertos cuando cerró el editor la última vez.
#. **Snippets:** te da la posibilidad de insertar trozos de código con una
   palabra clave. Hay una serie de snippes predeterminados, como por ejemplo,
   al crear un nuevo archivo en blanco, si uno escribe ``lorem``, y presionas
   ``Tab``. Deberías obtener un párrafo del texto de lorem ipsum.

Nota: También puedes crear tus propios snippets en: ``Tools`` > ``New Snippet``.
Mira `la documentación para obtener ayuda.
<http://docs.sublimetext.info/en/latest/extensibility/snippets.html>`_

Configuraciones personalizadas de Sublime Text
==============================================

Puede configurar completamente Sublime Text utilizando archivos de
configuración basados en JSON (para la configuración base y para
configuraciones específica para cada lenguaje de programación), por lo que es
muy fácil transferir o sincronizar tus configuraciones personalizada a otro
sistema.

Configuración general
*********************

Primero necesitamos crear nuestra configuración base de Sublime Text. Para
crear un archivo de configuración base, en Sublime Text, hay que hacer clic en
``Preferences`` > ``Settings - User``. Esto abre el archivo de las
preferencias predeterminadas (a la izquierda) y un archivo JSON vacío, para
escribir nuestras preferencias personalizadas en él (a la derecha, llamado
``Preferences.sublime-settings - User``).

Por ejemplo este es mi (mimimalista) ``Preferences.sublime-settings - User``:

.. code-block:: json

    // Settings in here override those in "Default/Preferences.sublime-settings"
    // and are overridden in turn by syntax-specific settings.
    {
        // Columns in which to display vertical rulers
        "rulers": [80],
        // Tabulation options.
        "tab_size": 4,
        "translate_tabs_to_spaces": true,
        "draw_white_space": "all",
        // Highlights.
        "draw_indent_guides": true,
        "highlight_line": true,
        // Used when saving new files
        "default_encoding": "UTF-8",
        "ensure_newline_at_eof_on_save": true,
    }

En esta configuración, establecimos las siguientes opciones:

1. El ``"rulers": [80]`` inserta una regla vertical en la columna 80. Para
   ayudar a la costumbre de intentar mantener el ancho de línea de 80
   caracteres o menos (para una mejor visualización en pantallas de terminales
   pequeños y al hacer diff entre archivos).
#. El ``"tab_size": 4``` establece la longitud de los tabuladores a unos 4
   espacios de largo (común en algunos lenguajes como Python) y el
   ``"translate_tabs_to_spaces": true`` convierte los tabuladores en espacios
   automáticamente cuando se presione la tecla tab |tecla-tab| .
#. El ``draw_white_space": "all"`` es para mostrar los caracteres invisibles
   (como espacios, tabs, etc.) en el texto. Otros valores posibles son:
   ``"selection"`` para dibujar los caracteres invisibles solo en el texto que
   está seleccionado y ``"none"`` para desactivar (no mostrar los caracteres
   invisibles).
#. Otros: ``"default_encoding": "UTF-8",`` fija la codificación predeterminada
   para los nuevos archivos en UTF-8 y ``"ensure_newline_at_eof_on_save": true``
   es útil en sistemas tipo unix. [2]_

Configuraciones especificas para cada lenguaje de programación
**************************************************************

En sublime text puedes establecer una configuración específica para cada
lenguaje de forma independiente.

Para configuraciones específicas de cada lenguaje, en Sublime Text haga clic
en: ``Preferences`` > ``Settings - More`` > ``Syntax Specific - User``. Luego
guarde el archivo generado usando el siguiente formato:
``LANGUAGE.sublime-settings``.

Obviamente puedes configurar tus ajustes a tu gusto. Sin embargo, les
recomiendo comenzar con la configuración que propongo y luego hacer los cambios
que consideren adecuados.

Preferencias para HTML y CSS:
_____________________________

Al inicio fijamos las tabulaciones en 4 espacios, pero para mi gusto, al
escribir en HTML prefiero una indentación de solo 2 espacios (debido a que el
HTML tiende a tener varios niveles de indentado y cuacquer valor mayor a 2
espacios tiende a empujar parte HTML pasado el borde de las 80 columas de las
pantallas tipo terminales).

Así que vamos a establecer un tamaño de tabulador diferente para el HTML. En
Sublime Text ve a: ``Preferences`` > ``Settings - More`` >
``Syntax Specific - User`` para crear una configuración vacía y luego copia la
siguiente configuración en el archivo vacío:

.. code-block:: json

    {
        // tabs and whitespace
        "draw_white_space": "all",
        "tab_size": 2
        // Automatically close HTML and XML tags when </ is entered
        "auto_close_tags": true,
    }

Luego guarda el archivo como ``HTML.sublime-settings`` (ve a ``file`` >
``save as`` y renombra el archivo como ``HTML.sublime-settings``, guárdalo y
ciérralo).

Para el CSS también vamos a dejar la longitud del tabulador en 2 espacios, para
ello vaya a: ``Preferences`` > ``Settings - More`` >
``Syntax Specific - User`` para crear una configuración vacía y luego copia la
siguiente configuración en el archivo vacío:

.. code-block:: json

    {
        // tabs and whitespace
        "draw_white_space": "all",
        "tab_size": 2
    }

Luego guarda el archivo como ``CSS.sublime-settings`` (ve a ``file`` >
``save as`` y renombra el archivo, guárdalo y ciérralo).

Preferencias para Python:
_________________________

Estas son mis configuraciones especificas para Python. En Sublime Text ve a:
``Preferences`` > ``Settings - More`` > ``Syntax Specific - User`` para crear
una configuración vacía y luego copia la siguiente configuración en el
archivo vacío:

.. code-block:: json

    {
        // tabs and whitespace
        "draw_white_space": "all",
        "auto_indent": true,
        "smart_indent": true,
        "tab_size": 4,
        "trim_automatic_white_space": true,
        "use_tab_stops": true,
        "word_wrap": true,
        "wrap_width": 80
    }

Luego guarda el archivo como ``Python.sublime-settings`` (ve a ``file`` >
``save as`` y renombra el archivo, guárdalo y ciérralo).

una buena referencia para las diferentes configuraciones la puedes encontrar
en la `documentación No-oficial de Sublime Text.
<http://sublime-text-unofficial-documentation.readthedocs.org/en/latest/reference/settings.html>`_

Administrador de Paquetes
=========================

Ahora es el momento de instalar algunos complementos y temas adicionales, pero
para ello primero tenemos que instalar el administrador de paquetes llamado
"Package Control". Una vez que lo tienes instalado, puede usarlo para instalar,
eliminar y actualizar otros paquetes ST3.

Instalación del administrador de Paquetes
*****************************************

To install the Package Control you have two alternatives:

1. Si tu tienes instalada una versión reciente [3]_ de Sublime text 3, ve
   a ``Tools`` > ``Install Package Control``
2. Si no tienes la opción anterior (o está usando una versión antigua de
   sublime text), simplemente tienes que abrir la consola de Sublime Text (menú
   ``View`` > ``Show Console``), ir a la `pagina de instalación de su sitio,
   <https://packagecontrol.io/installation>`_ y copiar un código Python algo
   extraño, peguar el código en la consola, presione ``Enter`` |tecla-enter|
   y ... ¡Presto! Ahora puedes instalar cualquier paquete fácilmente desde
   Sublime.

Después de instalarlo, podrás obtener paquetes directamente desde Sublime Text.
¡Olvídate de buscar e instalar cosas manualmente!

Uso del administrador de Paquetes
*********************************

1. Abre la paleta de Comandos: Presiona **Ctrl+Shift+P** (en Windows o
   GNU/Linux) o **Cmd (** ( |tecla-cdm-mac|**)+Shift+P** (en Mac OS X).
2. Escribe "Package Control" y selecciona "Package Control: Install Package".
3. Se va a mostrar una lista de los paquetes disponibles. Hace Doble-click en
   el nombre del paquete que quieres instala para iniciar su instalación.

UI y Temas
=============

Los temas son algo subjetivos, y normalmente evito recomendar uno. Sin embargo,
en Atom me gusta mucho el tema Seti y Sublime Text tiene una adaptación de ese
tema. Otros temas que personalmente me gustan son:

- `Seti UI (adaptación para ST3). <https://packagecontrol.io/packages/Seti_UI>`_
- `Soda Dark Theme <https://packagecontrol.io/packages/Theme%20-%20Soda>`_
- `Ayu Theme <https://packagecontrol.io/packages/ayu>`_
- `Dracula Theme <https://packagecontrol.io/packages/Dracula%20Color%20Scheme>`_
- `Flatland Theme. <https://packagecontrol.io/packages/Theme%20-%20Flatland>`_

Después de instalar un tema usando el control de paquetes (Package Control),
asegúrate de actualizar tu configuración base de Sublime Text ``Preferences`` >
``Settings - User`` y agrega las líneas del tema en tu configuración de
usuario, por ejemplo:

.. code-block:: json

    {
      "theme": "ayu-light.sublime-theme",
      "color_scheme": "Packages/ayu/ayu-light.sublime-color-scheme"
    }

Paquetes Generales
==================

Al igual que Atom, Sublime tiene muchos paquetes y temas. Para mi los más
útiles son:

SideBar Enhancements
********************

SideBarEnhancements amplía el número de opciones de menú en la barra lateral,
agregando algunas funciones clave a la barra lateral como abrir en el navegador,
copiar nombres y rutas, etc... lo cual acelera el flujo de trabajo en general.
Este tipo de características realmente deberían estar allí por defecto en
cualquier editor de texto.

- `SideBarEnhancements <https://packagecontrol.io/packages/SideBarEnhancements>`_

Bracket HighLighter
*******************

Este complemento proporciona una ayads visual para ubicar en donde comienza y/o
termina una etiqueta o paréntesis. Ayuda mucho, especialmente en la depuración
al resaltar los limites del texto.

- `Bracket HighLighter <https://packagecontrol.io/packages/BracketHighlighter>`_

Color HighLighter
*****************

HighLighter es un paquete para mostrar en el resaltado del texto de un código
hexadecimal, gba, rgba, hsl, hsla, etc. el color real que corresponde a ese
codigo.

Además, viene con su propio selector de color, solo presiona ctrl+Shift+C y
elige tu color.

- `Color HighLighter (project is abandoned?)
  <https://packagecontrol.io/packages/Color%20Highlighter>`_

- **Alternativa:** `Color Highlight
  <https://packagecontrol.io/packages/Color%20Highlight>`_

Code​ Formatter
***************

Code​ Formatter convierte el código desordenado (u optimizado) en uno más
ordenado y legible. Tiene soporte para los lenguajes de programación: HTML,
CSS, JavaScript, JSON, PHP, Python y VBScript.

- `CodeFormatter <https://packagecontrol.io/packages/CodeFormatter>`_

Linter
******

Sublime Linter es la "base" para los distintos paquetes de linter de ST3 (un
linter es un programa/script que busca errores en el código). Sublime Linter
proporciona la API que usan los linters de los lenguajes específicos.

Después de instalar este paquete principal, debe instalar el linter específico
para cada uno de los lenguajes de programación que quieres usar.

- `Sublime Linter <https://packagecontrol.io/packages/SublimeLinter>`_

Desarrollo Web
==============

Emmet
*****

Emmet (antes conocido como Zen Coding) es un plugin disponible para varios
editores de texto populares (incluyendo Sublime Text, Visual Studio, Eclipse,
Atom, etc.) este plugin te permite escribir código valido HTML sin tener que
escribir las etiquetas completas de HTML, sino usando las abreviaciones de
Emmet. Por ejemplo, puedes escribir la siguiente linea en tu editor:

.. code-block:: html

    div#content>ul#nav>li*4>a

Y tocar la tecla para "expandir la abreviación" de Emmet (por defecto la tecla
Ctrl+e). Entonces la abreviación se transforma mágicamente
en HTML valido:

.. code-block:: html

    <div id="content">
      <ul id="nav">
        <li><a href=""></a></li>
        <li><a href=""></a></li>
        <li><a href=""></a></li>
        <li><a href=""></a></li>
      </ul>
    </div>

- `Emmet <https://packagecontrol.io/packages/Emmet>`_

LiveReload
**********

Un complemento de recarga de página sublime text 3.

- `LiveReload <https://packagecontrol.io/packages/LiveReload>`_

AutoPrefixer
************

Solo ejecuta esto al escribir una propiedad de CSS y se agregará
automáticamente cada prefijo CSS. ¡Simple y rápido!

- `AutoPrefixer <https://packagecontrol.io/packages/Autoprefixer>`_

Minify
******

Minify para Sublime Text te permite rápidamente reducir (minify) u ordenar
código de JavaScript, JSON, HTML y SVG

- `Minify <https://packagecontrol.io/packages/Minify>`_

- **Alternativa** `Minifier: <https://packagecontrol.io/packages/Minifier>`_
  para archivos JS y CSSs

linters para CSS y JS
*********************

Reporte errores en el código CSS y js (requiere Sublime Linter)

- `linter css <https://github.com/SublimeLinter/SublimeLinter-csslint>`_
- `linter JSHint <https://packagecontrol.io/packages/JSHint>`_

Paquetes para Python
====================

linters: flake8 y pydocstyle
****************************

A continuación vamos a instalar un linter de Python, para que nos ayude a
detectar errores en nuestro código escrito en python. Hay varios, pero yo
recomiendo uno llamado linter-flake8 que usa por debajo el conocido flake8 (En
pocas palabras, flake8 es un el envoltorio que verifica a la vez: el
cumplimiento del `pep8, <https://github.com/SublimeLinter/SublimeLinter-pep8>`_
revisa el código como lo hace `pyflakes
<https://github.com/SublimeLinter/SublimeLinter-pyflakes>`_ y revisa la
complejidad circular")

- `linter-flake8 <https://packagecontrol.io/packages/SublimeLinter-flake8>`_

Si instalaste el linter-flake8, ya tienes una validación automática contra
el PEP8, pero falta otro paquete que es necesario para validar las cadenas
de documentación (docstrings) de acuerdo a la semántica del PEP 257. Esto se
resuelve instalando el linter-pydocstyle que puede ser usado lado a lado con
flake8 .

- `linter-pydocstyle <https://packagecontrol.io/packages/SublimeLinter-pydocstyle>`_

Otra alternativa interesante del paquete es Pylint, que es una herramienta para
verificar los módulos y paquetes utilizados que varios archivos a la vez en un
proyecto.

- `linter-pylint <hhttps://github.com/SublimeLinter/SublimeLinter-pylint>`_

Anaconda
********

Otra opción es Anaconda que agrega una serie de características similares a un
IDE a ST3, incluyendo lo siguiente:

- **Autocompletar:** funciona de forma predeterminada, pero tiene una serie de
  configuraciones opcionales.
- **Buscar:** busca rápidamente donde se ha usado una variable, función o clase
  en un archivo específico.
- **Ir a la Definición:** encuentra y muestra la definición de cualquier
  variable, función, o clase a lo largo de todo tu proyecto.
- **linting**: usando PyLint o PyFlakes con PEP8. Personalmente uso un paquete
  diferente de linter (mencionado anteriormente), así que deshabilito el
  lintter por completo en el archivo de configuración de Anaconda definido por
  el usuario (``Anaconda.sublime-setting``), en el menú: ``Preferences`` >
  ``Package Settings`` > ``Anaconda`` > ``Settings - User`` agregando la linea
  ``{"anaconda_linting": false}`` en el erchivo ``Anaconda.sublime-setting``.
- **Comprobar la complejidad de código de McCabe:** Ejecuta el test de `McCabe
  de complexity <https://es.wikipedia.org/wiki/Complejidad_ciclom%C3%A1tica>`_
  en un archivo especifico.
- **Mostrar la Documentación:** muestra la cadena de documentación de las
  funciones o clases (si está escrita, por supuesto).

Bonos Extras
============

- `requirements.txt <https://packagecontrol.io/packages/requirementstxt>`_
  proporciona un autocompletado y resaltado de sintaxis, así como un buen
  sistema de administración de versiones para sus archivos "Requirements.txt".
- Todos los `plugins oficiales de linter para
  SublimeLinter <https://github.com/SublimeLinter>`_
- `Markdown Preview <https://packagecontrol.io/packages/Markdown%20Preview>`_
  Se utiliza para previsualizar y traducir archivos de markdown.

**Notas:**

.. [1] Un ejemplo de esto es la `documentación No-oficial de Sublime
   Text <docs.sublimetext.info/>`_ que es mejor (y más completa) que la
   documentación oficial.

.. [2] de acuerdo a POSIX, cada archivo de texto consiste de una serie de
   lineas, en donde cada una de ellas debe terminar con el carácter de nueva
   linea (``\n``), incluyendo la ultima linea.

   Algunos programas tienen problemas para procesar un archivo si este no
   termina en una nueva línea. Por ejemplo, GCC advierte sobre esto, no porque
   no pueda procesar el archivo, sino porque tiene que hacerlo para cumplir
   con el estándar.

   Referencia: `El archivo e e-mails de GCC/GNU.
   <http://gcc.gnu.org/ml/gcc/2003-11/msg01568.html>`_ como el estandar
   POSIX `define una linea (ver 3.206 Line).
   <http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap03.html#tag_03_206>`_

.. [3] SublimeText 3 `Versión 3124 o superior.
   <https://www.sublimetext.com/blog/articles/sublime-text-3-build-3124>`_

.. |tecla-tab| unicode:: U+21B9
.. |tecla-cdm-mac| unicode:: U+2318
.. |tecla-enter| unicode:: U+21B5
