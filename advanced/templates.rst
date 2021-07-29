Templates
=========

.. include:: ../_includes/contents.rst

Thanks to the flexible HOCON format you can create templates for anything. A template can be any regular parameter, :ref:`hocon-obj`, :ref:`hocon-list`, etc. Templates can be set both inside the menu file, or you can put all templates in a separate file. 

Templates basics
----------------

For example, in each of the bunch of menus the "Close menu" button has the same look and parameters. Lets create this button, but not outside of default ``items`` list. Example:

::

	closeButton {
	  slot: 5
	  texture: "5a6787ba32564e7c2f3a0ce64498ecbb23b89845e5a66b5cec7736f729ed37"
	  name: "&cClose"
	  lore: "&7Click to close the menu"
	  click {
	    closeMenu: true
	  }
	}

Since this item outside of items list, this is a template.

Now you can include this template in any place of the menu. For this, you need a special placeholders. This placeholder has format:

::

	${<template_name>}

Replace ``<template_name>`` to name of the your template block. In our case this is a ``closeButton``. Lets add this template to the ``items`` list:

::

	items: [
	  ${closeButton}
	]

Now after plugin reload, this item will appear in menu.

The main feature of templates if that you can use them multiple times. But now our template has static slot. To change slot we need to override this in place where we include it.

Expand or override template
---------------------------

Override objects
~~~~~~~~~~~~~~~~

To override our object, we need to use this syntax:

::

	items: [
	  ${closeButton} {
	    slot: 0
	  }
	]

After template placeholder we opened brackets as in default object. And then added item properties as always.

You can add or override any exists properties:

::

	items: [
	  ${closeButton} {
	    slot: 0
	    name: "Peter"
	    glow: true
	  }
	]

Override lists
~~~~~~~~~~~~~~

To override list, you need to use similar syntax. For example, we need to set background for menu:

::

	// Menu's items list
	items: ${myTmpl} [
	  ${closeButton} {
	    slot: 0
	  }
	]

	// Template
	myTmpl: [
	  {
	    slot: "0-53"
	    material: STAINED_GLASS_PANE
	    name: " "
	  }
	]

Templates in separate file
--------------------------

Templates in shared file useful to reuse your templates in multiple menus.

In order to tell the plugin that file is a templates file, you have to specify ``#invisible`` tag in the first line, otherwise your templates file will be detected by the plugin as a menu file and will produce error.

Below is an example of shared template file:

::

	#invisible

	btnClose {
	  texture: "5a6787ba32564e7c2f3a0ce64498ecbb23b89845e5a66b5cec7736f729ed37"
	  name: "&cClose"
	  click {
	    closeMenu: true
	  }
	}

	btnBack {
	  material: ARROW
	  name: "&cBack"
	}

To use this templates you need to include this file inside menu file. For this, you need to use this command:

::

	include required(file("./plugins/AbstractMenus/menus/<path_to_file>"))

You need to replace ``<path_to_file>`` to full path to your template file begins from ``menus`` folder.

In our case, templates file placed in the root of ``menus`` folder. Then menu file will looks like this:

::

	include required(file("./plugins/AbstractMenus/menus/templates.conf"))

	title: "Menu title"
	size: 6
	items: [
	  {
	    slot: 0
	    material: STONE
	    name: "Some item"
	  },
	  ${btnClose} { slot: 1 }
	]