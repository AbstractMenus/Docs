HOCON format
============

.. contents:: Contents
   :depth: 3

To configure menus, AbstactMenus uses **HOCON** (`.conf` files) instead of **YAML** (`.yml` files). This format may seem complicated for you, but later you will understand why this format is ideal for really flexible menus.

.. note:: More information about HOCON format you can find `here <https://github.com/lightbend/config/blob/master/HOCON.md>`_.

Basic HOCON syntax
------------------

In general, HOCON is a modified and optimized for configs JSON format. So if you know JSON, then HOCON will seem familiar for you.

Key-value
~~~~~~~~~

All data in HOCON has this format: ::

	<key>: <value>

where:

:<key>: Just unique key name, like in YAML
:<value>: Some value. Value may be one of the described below data type.

A few examples:

::

	name: "AbstractMenus"

::

	age: 20

In this examples ``name`` and ``age`` are **keys**. ``"AbstractMenus"`` and ``20`` are **values**. More about data types see topic :ref:`data-types`

Comments
~~~~~~~~

Comments are invisible for plugin. So you can comment some lines for yourself. Comments can start with ``//`` or ``#`` symbols.

::

	# This is my first comment
	// This is my another comment

.. _data-types:

HOCON data types
----------------

When we write some actions, rules, activators and other things, we use some data types to describe this. This data can be ``String``, ``Number``, ``Boolean``, ``Object`` and ``List``. Each data type has its own writing format. Below we described all the data types that you can use when writing menus.

String
~~~~~~

The ``String`` format is simple. Below is example how you can specify this.

::

	param_name: "Some text"

In HOCON, the text is enclose between double quotes ``"``, for example:

::

	name: "Peter Piper"

If there are no spaces or specific characters in the text, you can write it without quotes:

::

	name: Peter

.. important:: Unlike YAML, where text can be specified in single quotes ``'``, this is not possible in HOCON. The text can be written only between double quotes or without them.

Number
~~~~~~

The ``Number`` in HOCON can be specified in same way as in YAML:

::

	count: 5
	age: 21

Floating-point numbers writes through a dot, like this:

::

	x: 224.5
	y: 16.0

Boolean
~~~~~~~

The ``Boolean`` type (``true`` or ``false``) can be specified in the same way as in YAML:

::

	glow: true
	unbreakable: false

.. _hocon-obj:

Object
~~~~~~

The ``Object`` type (also sometimes called ``block`` in this docs) in HOCON is parameter, that containing other parameters grouped together. In HOCON, an Object can be specified between braces ``{}``. Before the brackets should be the name of the object. For example:

::

	item {
	  material: IRON_SWORD
	  name: "&eMy super sword"
	}

In this example, we described the item object (button). This contains such parameters as ``material`` and ``name``. You can omit a colon before the brackets. So this:

::

	item: {

and this:

::

	item {

are both correct ways to start object. 

Each object can contain other objects inside and so on ad infinitum. For example:

::

	item {
	  material: LEATHER_BOOTS
	  color {
	    r: 255
	    g: 255
	    b: 255
	  }
	}

Here we specified the ``color`` object inside the ``item`` object. This is just an example. There is a separate lesson on how to describe items in the plugin - :doc:`../general/item_format`.

.. _hocon-list:

List
~~~~

The ``List`` in HOCON is a very flexible type. The list may contain any of the data types listed above, but only one type per one list.
Creating a list is something like creating an object, but using a colon and square ``[]`` brackets:

::

	list: []

This is empty list. You can fill this with data of various types. Below are the lists that you will use in the plugin.

.. _hocon-list-str:

Strings List
""""""""""""

::

	lore: [
	  "Line 1",
	  "Line 2"
	]

A comma ``,`` is placed after each element in the list. This means that there is another element after comma. You can omit the comma after last element.

.. _hocon-list-num:

Numbers List
""""""""""""

The list of numbers is written in the same way as the list of strings, but without quotes:

::

	numbers: [
	  52,
	  12,
	  36
	]

.. _hocon-list-obj:

Objects List
""""""""""""

As we already learned, the object must be specified between braces ``{}``. This rule also works for List. The one difference for lists is that objects must be specified without name. For example:

::

	items: [
	  {
	    material: STONE
	    name: "Pebble"
	  },
	  {
	    material: IRON_SWORD
	    name: "Excalibur"
	  }
	]

This is a list of items. Each object, as written earlier, is enclosed between braces. As list requires, each element inside should be separated by comma.

.. _hocon-list-feature:

Useful feature of the lists
~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you have only one element inside list, you can specify it as regular parameter.

For example, you have list of strigns and only strign inside. Then this:

::

	regionJoin: [
	  "my_region"
	]

can become this:

::

	regionJoin: "my_region"

Or if you have list of objects, the this:

::

	items: [
	  {
	    material: STONE
	    name: "Pebble"
	  }
	]

can become this:

::

	items {
	  material: STONE
	  name: "Pebble"
	}

In the next lessons, you will learn more about numerous menu features, and examples of their use. This article will help you better understand future examples and write first interactive menu yourself using AbstractMenus.