Colorizing text
===============

.. include:: ../_includes/contents.rst

Colors codes
------------

As in most plugins, AM replaces ``&`` char to native Minecraft ``§`` color prefix. All text can be colorized. There is some examples below.

::

	message: "&aSome &etext"

You can use this cheatsheet to get required color code.

.. figure:: ../_static/colors.png
	:align: center

	Default color codes

RGB colors
----------

Since Spigot 1.14 text can be colorized with RGB colors via hex code. All RGB colors must be concluded between ``<>`` brackets. Example:

::

	message: "<#00FF00>Some text"

This text will be colorized into green color.

RGB colors can also be combined with color codes. Example:

::

	message: "<#00FF00>Some &etext"