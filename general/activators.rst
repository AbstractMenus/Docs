.. include:: ../_includes/datatypes.rst

.. |ex_below| replace:: See example

Activators
==========

.. include:: ../_includes/contents.rst

Activator is an events that causes menu opening. 

All activators must be inside the ``activators`` block in menu root along with parameters such a ``title`` and ``size``. You can specify one or several activators, or you can make menu without activators. Example:

::

	activators {
	  command: [ // Open menu by one of the command in list
	    "menu",
	    "game"
	  ]
	  join: true // Also open menu on joining to server
	}

In this example, the menu will be opened when you joined to the server, or if you enter ``/menu`` or ``/game`` command.

.. tip:: Many activators, such a ``command`` saves their state to get values and use them in menu. More about this cool feature read in :doc:`../advanced/input` article.

.. _activators-all:

All activators
--------------

.. csv-table::
	:header: "Name", "Data type", "Example", "Description"
	:widths: 5, 5, 10, 30

	"command", Multiple types, |ex_below| :ref:`activ-cmd`, "Open menu when command entered"
	"chat", |t_list_str|, |ex_below| :ref:`activ-chat`, "Open menu when entered some chat message"
	"containsChat", |t_list_str|, |ex_below| :ref:`activ-chat`, "Open menu when chat message contains some word"
	"join", |t_bool|, ``join: true``, "Open when player join the server "
	"regionJoin", |t_list_str|, |ex_below| :ref:`activ-wg`, "Open menu when player joined some WG region"
	"regionLeave", |t_list_str|, |ex_below| :ref:`activ-wg`, "Open menu when player leaved some WG region"
	"clickItem", |t_list_obj|, |ex_below| :ref:`activ-item`, "Open menu when RMB click on the item in the hand"
	"clickNPC", |t_list_int|, |ex_below| :ref:`activ-npc`, "Open menu when RMB click on NPC (from Citizens)"
	"clickEntity", |t_list_obj|, |ex_below| :ref:`activ-entity`, "Open menu when RMB click on entity"
	"shiftClickEntity", |t_list_obj|, |ex_below| :ref:`activ-entity-shift`, "Open menu when Shift-RMB click on entity"
	"clickBlock", |t_list_obj|, |ex_below| :ref:`activ-block`, "Open menu when some block in specified location clicked"
	"clickBlockType", |t_list_str|, |ex_below| :ref:`activ-block-type`, "Open menu when some block of specified type clicked"
	"button", |t_list_obj|, |ex_below| :ref:`activ-btn`, "Open menu when button clicked"
	"lever", |t_list_obj|, |ex_below| :ref:`activ-btn`, "Open menu when lever shifted"
	"plate", |t_list_obj|, |ex_below| :ref:`activ-btn`, "Open menu when plate activated"
	"table", |t_list_str|, |ex_below| :ref:`activ-sign`, "Open menu when sign with some text clicked"
	"swapItems", None, |ex_below| :ref:`activ-swap-items`, "Open menu when player swaps item. By default it's 'F' key"


.. _activ-cmd:

Activator ``command``
---------------------

**Extractor type**: :ref:`extractor-cmd`

Open menu if player entered command. This activator has several formats which described below.

.. note:: Commands registering occurs during server startup. If you change command's base name and reload plugin with ``/am reload``, tab complete for new created command may not work. To make it work, you need to reload server. All other changes in commands, like changing arguments will work correctly.

Single command
~~~~~~~~~~~~~~

Just single command name without arguments. Example:

::

	command: "menu"

Command with aliases
~~~~~~~~~~~~~~~~~~~~

You can add aliases to your command. For this, you need to write ``command`` activator as list of strings. Example:

::

	command: [
	  "menu",
	  "game"
	]

When you enter ``/menu`` or ``/game`` command, menu will be opened.

Command with arguments
~~~~~~~~~~~~~~~~~~~~~~

This activator has other, more complex formats. You can build own commands with arguments. See :ref:`building-commands` topic to know how to do this.

.. _activ-chat:

Activator ``chat``
------------------

**Extractor type**: *None*

Full word or phrase
~~~~~~~~~~~~~~~~~~~

This is a list in which a word or words are specified, when you enter one of them in the chat, menu will be opened. Example:


::

	chat: [ // Ope menu when entered to chat one of the messages
	  "hello",
	  "open the menu"
	]

Part of a word, phrase, symbol
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This is a list in which you can specify words, phrases, etc. Menu will be opened if entered message contains at least one specified word, phrase or symbol. Example:

::

	containsChat: [
	  "hey",
	  "menu",
	  "or"
	]

In this example, if player's message contains ``hey``, ``menu`` or ``or`` symbols together, then menu will be opened.

.. _activ-wg:

WorldGuard regions
------------------

**Extractor type**: :ref:`extractor-region`

These activators can only be used with the `WorldGuard <https://dev.bukkit.org/projects/worldguard>`_ plugin. 
Also you need to enable ``worldGuard`` parameter in the AbstractMenus general config.

Activator ``regionJoin``
~~~~~~~~~~~~~~~~~~~~~~~~

Here listed regions which will open the menu when entering into them. Example:

::

	regionJoin: [  // Open a menu when enter region
	  "spawn",
	  "otherRegion"
	]

In this example, the menu will be opened if you enter to ``spawn`` or ``otherRegion`` region.

Activator ``regionLeave``
~~~~~~~~~~~~~~~~~~~~~~~~~

Here listed regions which will open the menu when leaving a region. Example:

::

	regionLeave: [  // Open menu when leaving from region
	  "pvp",
	  "someRegion"
	]

In this example, the menu will be opened if you leave from ``spawn`` or ``otherRegion`` region.

.. _activ-item:

Activator ``clickItem``
-----------------------

**Extractor type**: :ref:`extractor-item`

You can add activator to open menu when some item clicked by right click in player's hand.

::

	clickItem {
	  material: STONE
	  name: "Open menu"
	}

.. tip:: Make sure that you specified all item properties. If some property missing, plugin won't handle click on required item.

.. _activ-npc:

Activator ``clickNPC``
----------------------

**Extractor type**: :ref:`extractor-npc`

Here listed NPC id which will open the menu when click NPC. Example:

::

	clickNPC: [
	  1,
	  23
	]

In this example, the menu will be opened if you clicked on the NPC with id ``1`` or ``23``.

.. tip:: To find NPC id just type ``/npc sel`` while you looking at NPC. After this enter ``/npc`` command.

Entity clicks
-------------

Open menu by clicking on entity. There are two types of this activator: for simple clicks and clicks with ``Shift`` key pressed.

.. tip:: All entity types can be found `here <https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/entity/EntityType.html>`_.

.. _activ-entity:

Activator ``clickEntity``
~~~~~~~~~~~~~~~~~~~~~~~~~

**Extractor type**: :ref:`extractor-entity`

The ``clickEntity`` activator is a :ref:`list of objects <hocon-list-obj>`. Each object is a simple entity data. Example:

::

	clickEntity {
	  type: ZOMBIE
	  name: "&eZombie"
	}

If you want to add multiple entities, you can use this activator as list. Example:

::

	clickEntity: [
	  {
	    type: ZOMBIE
	    name: "&eZombie"
	  },
	  {
	    type: PLAYER
	  }
	]

Each object have parameters:

:type: Bukkit's type of the entity.
:name: [Optional]. Display name of the entity.

In this example we specified ``PLAYER`` entity and ``ZOMBIE`` entity with ``&eZombie`` name. If player clicked on any player or zombie named ``&eZombie``, menu will be opened. 

.. _activ-entity-shift:

Activator ``shiftClickEntity``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Extractor type**: :ref:`extractor-entity`

This activator format similar to :ref:`activ-entity`. The only difference is that this activator will open menu only if player sneaking (usually by ``Shift`` key) while click on entity. Example:

::

	shiftClickEntity {
	  type: PLAYER
	}

If you want to add multiple entities, you can use this activator as list. Example:

::

	shiftClickEntity: [
	  {
	    type: ZOMBIE
	    name: "&eZombie"
	  },
	  {
	    type: PLAYER
	  }
	]

Block click
-----------

Block click activators handles clocking on block (right and left click). THere are two types of this activator, to check click by block location, or block type (material).

.. _activ-block:

Activator ``clickBlock``
~~~~~~~~~~~~~~~~~~~~~~~~

**Extractor type**: :ref:`extractor-block`

In this code block you can specify location of some world's block. If player click on this block, menu will be opened.

::

	clickBlock {
	  world: "world"
	  x: 0.0
	  y: 0.0
	  z: 0.0
	  yaw: 0.0
	  pitch: 0.0
	}

And short version:

::

	clickBlock: "world, 0.0, 0.0, 0.0, 0.0, 0.0"

This version has format:

::

	clickBlock: "<world>, <x>, <y>, <z>, <yaw>, <pitch>"

There is ability to specify several locations, since ``clickBlock`` is a list.

::

	clickBlock: [
	  "world, 0, 0, 0",
	  "world, 1, 1, 1",
	]

.. _activ-block-type:

Activator ``clickBlockType``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Extractor type**: :ref:`extractor-block`

In this code block you can specify type of some world's block. If player click on block of specified type, menu will be opened. Example:

::

	clickBlockType: STONE

If you want to add multiple block types, you can use this activator as list:

::

	clickBlockType: [
	  STONE,
	  CAKE
	]

On servers with version 1.12.2 or lesser you can use numeric ids.

.. _activ-btn:

Activators ``button``, ``lever``, ``plate``
-------------------------------------------

**Extractor type**: :ref:`extractor-block`

A ``button``, ``lever`` and ``plate`` activators has same format. Below is example for buttons:

::

	button {
	  world: "world"
	  x: 0.0
	  y: 0.0
	  z: 0.0
	}

The location, as in other places, can be specified in one line:

``button: "<x>, <y>, <z>"`` - The world will default (``world``). Yaw and Pitch are zero.

``button: "world, <x>, <y>, <z>"`` - Yaw and Pitch are zero.

``button: "world, <x>, <y>, <z>, <yaw>, <pitch>"`` - Includes all location parameters.

There is ability to specify several locations, since ``button``, ``lever`` and ``plate`` is a lists.

::

	plate: [
	  "world, 0, 0, 0",
	  "world, 3, 1, 8",
	]

.. _activ-sign:

Activator ``table`` (sign)
--------------------------

**Extractor type**: *None*

The ``table`` activator is a :ref:`strings list <hocon-list-str>`

::

	table: [
	  "[OPEN]"
	]

In this example, the label in the first line should be the text ``[OPEN]``.

::

	table: [
	  "[OPEN]"
	  ""
	  "menu"
	]

In this example, the label in the first line should be the text ``[OPEN]``, and on the third a ``menu``.

.. _activ-swap-items:

Activator ``swapItems``
-----------------------

**Extractor type**: *None*

This activator opens menu if player press key to swap items. By default this key is ``F``. 
Since this activator has no any arguments, we will use just ``true`` value. Example:

::

	activators {
	  swapItems: true
	}

.. note:: This activator works only for MC 1.9+