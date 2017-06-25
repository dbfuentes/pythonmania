+++
title = "Zip files in Python"
slug = "zip-files-in-python"
date = "2017-06-25T15:56:41-04:00"
categories = ["examples"]
tags = ["python2", "python3"]
author = "Daniel Fuentes"
draft = false
lang = "en"
+++

In this post I will explain how to handle a zip file using standard python
libraries.

<!--more-->

Creating a New Zip File
=======================

To create a new archive, simple instantiate the ZipFile with a mode of ``"w"``.
To add files, use the ``write()`` method.

By default, the contents of the archive are not compressed
(``zipfile.ZIP_STORED``). To add compression, the zlib module is required.

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

Getting the zip file information
================================

Testing if it is a valid zip file
---------------------------------

The ``is_zipfile()`` function returns a boolean indicating whether or not the
filename passed as an argument refers to a valid ZIP file (based on its `magic
number <https://en.wikipedia.org/wiki/Magic_number_(programming)>`_ ).

.. code-block:: python

    #!/usr/bin/env python

    import zipfile

    zipfilename = ["example.zip", "example2.txt", "example3.zip"]

    for filename in zipfilename:
        print(str(filename) + " " + str(zipfile.is_zipfile(filename)))

If you execute the script you get:

.. code-block:: python

    example.zip True
    example2.txt False
    example3.zip True

If the file does not exist at all, ``is_zipfile()`` returns False.

Read the names of the files in an ZIP File
------------------------------------------

To read the names of the files in an existing archive, use ``namelist()``

.. code-block:: python

    #!/usr/bin/env python

    import zipfile

    zf = zipfile.ZipFile("example.zip", "r")
    print(zf.namelist)
    zf.close()

The return value is a list of strings with the names of the archive contents:

.. code-block:: python

    ['test.txt']

Reading Meta-data from a ZIP Archive
------------------------------------

To access all of the meta-data about the ZIP contents, use the ``infolist()``
or ``getinfo()`` methods.

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

Which returns the information of the files inside the zip:

.. code-block:: python

    test.txt
        Comment:
        Modified: 2016-09-20 23:08:46
        System: 3 (0=MS-DOS OS-2, 3=Unix)
        ZIP version: 63
        Compressed: 548 bytes
        Uncompressed: 1278 bytes

There are additional fields other than those printed here, but deciphering the
values into anything useful requires careful reading of the `ZIP file
specification <http://www.pkware.com/documents/casestudies/APPNOTE.TXT>`_.

Extracting Archived Files From a ZIP Archive
============================================

To unzip a file simply do the following:

.. code-block:: python

    #!/usr/bin/env python

    import zipfile

    zipfilename = "test.zip"
    password = None

    # open and extract all files in the zip
    z = zipfile.ZipFile(zipfilename, "r")
    try:
        z.extractall(pwd=password)
    except:
        print('Error')
        pass
    zf.close()

Notes and Limitations:
======================

1. The zipfile module does not support ZIP files with appended comments, or
   multi-disk ZIP files. It does support ZIP files larger than 4 GB that use
   the ZIP64 extensions.

#. zipfile only supporting traditional PKWARE encryption method, if you try a
   WinZip AES-256 encrypted zip, ``zipfile.extractall`` raises
   a RuntimeError "Bad password for file" when given the correct password.
