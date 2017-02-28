+++
title = "Cambiando el user-agent de urllib"
slug = "cambiando-el-user-agent-de-urllib"
date = "2010-04-13T14:39:00-04:00"
categories = ["ejemplos"]
tags = ["python2", "web"]
author = "Daniel Fuentes"
draft = false
lang = "es"
+++

Hace algunos días necesitaba cambiar el `user
agent <http://es.wikipedia.org/wiki/Agente_de_usuario>`_ de urllib (o
sea como se identifica urllib al abrir una pagina web) para usarlo en `mi
script <http://logicerror.wordpress.com/2007/08/12/script-en-python-para-descargar-los-archivos-de-goear/>`_\ `para
bajar archivos desde goear <http://bitbucket.org/dbfuentes/goear-dl/>`_.
El problema es que normalmente a algunos sitios como google o la wikipedia no
les gusta que uno use otras cosas que nos `sean navegadores
validos <http://es.wikipedia.org/wiki/Navegador_web>`_ para ver sus paginas.

<!--more-->

Por ejemplo al hacer una búsqueda en google:

.. code-block:: python

    from urllib import urlopen
    pag = urlopen("http://www.google.com/search?q=python")
    print pag.read()

Nos responde mucho texto (en realidad es el html de una pagina de
error), que nos dice :

.. code-block:: php

    [...]<H1>Forbidden</H1>Your client does not have permission to get URL <code>/search?q=python</code> from this server.[...]

Ahora si abrimos el interprete en modo interactivo y hacemos:

.. code-block:: python

    >>> from urllib import URLopener
    >>> URLopener.version
    'Python-urllib/1.17'

Vemos que por defecto urllib se identifica al abrir alguna pagina (o
sea su user-agent) es *"Python-urllib/1.17"*.

Para cambiarlo por otro valor, consultamos la `documentacion de
python <http://docs.python.org/library/urllib.html#urllib._urlopener>`_
que nos dice:

    To override this functionality, programmers can create a subclass of
    URLopener or FancyURLopener, then assign an instance of that class
    to the urllib.\_urlopener variable before calling the desired
    function.

Con esto en mente hacemos una pequeña clase que hace el cambio, en
donde le señalamos un nuevo user agent (en el ejemplo voy a usar el de
IE7, aunque si quieren otros `pueden buscar por
aquí <http://www.useragentstring.com/pages/useragentstring.php>`_),
quedando el ejemplo de esta manera:

.. code-block:: python

    from urllib import FancyURLopener

    class MyOpener(FancyURLopener):
        version = 'Mozilla/5.0 (Windows; U; MSIE 7.0; Windows NT 6.0; en-US)'

    myopener = MyOpener()
    pag = myopener.open("http://www.google.com/search?q=python")
    print pag.read()

Y ahora si nos responde (nos muestra el html de la pagina) con un lindo:

.. code-block:: php

    [...]Results <b>1</b> - <b>10</b> of about <b>36,400,000</b> for <b>python</b> [...]

Claro que se puede usar con cualquier otro metodo de urllib, como por
ejemplo con retrieve (para descargar la pagina con los resultados de
la búsqueda, en vez de ver el html):

.. code-block:: python

    from urllib import FancyURLopener

    class MyOpener(FancyURLopener):
        version = 'Mozilla/5.0 (Windows; U; MSIE 7.0; Windows NT 6.0; en-US)'

    myopener = MyOpener()
    myopener.retrieve("http://www.google.com/search?q=python", "resultados.html")

Divertido, ¿o no?
