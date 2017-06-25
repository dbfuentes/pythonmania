+++
title = "Manejando archivos zip en Python"
slug = "archivos-zip-en-python"
date = "2017-06-25T15:55:28-04:00"
categories = ["ejemplos"]
tags = ["python2", "python3"]
author = "Daniel Fuentes"
draft = false
lang = "es"
+++

En esta entrada voy a explicar cómo manejar un archivo zip utilizando la
biblioteca estándar de python

<!--more-->

Crear un nuevo archivo Zip
==========================

Para crear un nuevo archivo, hay que usar una instancia de ZipFile en modo de
escritura ``"w"`` y para agregar archivos, use el método ``write()``.

De forma predeterminada, el contenido del archivo zip no está comprimido
(``Zipfile.ZIP_STORED``). Para añadir compresión, se requiere usar el módulo
zlib.

.. code-block:: python

    import zipfile
    try:
        import zlib
        compression = zipfile.ZIP_DEFLATED
    except:
        compression = zipfile.ZIP_STORED

    zf = zipfile.ZipFile("example.zip", mode="w")

    try:
        zf.write("test.txt", compress_type=compression)
    finally:
        zf.close()

Obtener la información del archivo zip
======================================

Comprobar si es un archivo zip válido
--------------------------------------

La función `` is_zipfile () `` devuelve un valor booleano (verdadero o falso)
que indica si el archivo es un archivo ZIP válido (basado en su `número magico
<https://es.wikipedia.org/wiki/N%C3%BAmero_m%C3%A1gico_(inform%C3%A1tica)>`_ ).

.. code-block:: python

    #!/usr/bin/env python

    import zipfile

    zipfilename = ["example.zip", "example2.txt", "example3.zip"]

    for filename in zipfilename:
        print(str(filename) + " " + str(zipfile.is_zipfile(filename)))

Si ejecutas el script obtienes:

.. code-block:: python

    example.zip True
    example2.txt False
    example3.zip True

Si el archivo no existe (o no es valido), ``is_zipfile()`` devuelve como
respuesta ``False``.

Leer los nombres de los archivos en un archivo ZIP
--------------------------------------------------

Para leer los nombres de los archivos que estan dentro de un archivo zip
existente, hay que usar ``namelist()`` de esta manera:

.. code-block:: python

    #!/usr/bin/env python

    import zipfile

    zf = zipfile.ZipFile("example.zip", "r")
    print(zf.namelist)
    zf.close()

El valor devuelto es una lista con los nombres del contenido del archivo zip:

.. code-block:: python

    ['test.txt']

Lectura de los metadatos de un archivo ZIP
------------------------------------------

Para acceder a todos los metadatos sobre el contenido ZIP, utilice el
comando ``infolist()`` y el método ``getinfo()``, por ejemplo.

.. code-block:: python

    #!/usr/bin/env python

    import datetime
    import zipfile

    zf = zipfile.ZipFile("example.zip", "r")

    for info in zf.infolist():
        print(info.filename)
        print("  Comment: " + str(info.comment))
        print("  Modified: " + str(datetime.datetime(*info.date_time)))
        print("  System: " + str(info.create_system) + " (0=MS-DOS OS-2, 3=Unix)")
        print("  ZIP version: " + str(info.create_version))
        print("  Compressed: " + str(info.compress_size) + " bytes")
        print("  Uncompressed: " + str(info.file_size) + " bytes")
    zf.close()

Que regresa la information de todos los archivos del zip:

.. code-block:: python

    test.txt
        Comment:
        Modified: 2016-09-20 23:08:46
        System: 3 (0=MS-DOS OS-2, 3=Unix)
        ZIP version: 63
        Compressed: 548 bytes
        Uncompressed: 1278 bytes

Hay campos adicionales distintos de los mostrados aquí, pero para poder
descifrar esos valores en cualquier cosa útil se requiere una cuidadosa
lectura de la `Especificación  de los archivos
ZIP <http://www.pkware.com/documents/casestudies/APPNOTE.TXT>`_.

Extraer archivos de un ZIP
==========================

Para descomprimir un archivo simplemente hay que hacer lo siguiente:

.. code-block:: python

    #!/usr/bin/env python

    import zipfile

    zipfilename = "example.zip"
    password = None

    # open and extract all files in the zip
    z = zipfile.ZipFile(zipfilename, "r")
    try:
        z.extractall(pwd=password)
    except:
        print('Error')
        pass
    zf.close()

Notas y Limitaciones:
=====================

1. El módulo zipfile no admite archivos ZIP con comentarios agregados, o
   archivos ZIP multi-disco. Pero soporta archivos ZIP de más de 4 GB que
   utilizan las extensiones ZIP64.

#. Zipfile sólo soporta el método tradicional de cifrado PKWARE, si se intenta
   un cifrado de WinZip AES-256, al usar ``zipfile.extractall`` resulta en un
   RuntimeError "Bad password for file" aun cuando se ingrese la contraseña
   correcta.
