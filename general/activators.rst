.. include:: ../_includes/datatypes.rst

.. |ex_below| replace:: See example

Activators
==========

.. include:: ../_includes/contents.rst

Activator is an events that causes menu opening. Activators always specified in menu root along with parameters such a ``title`` and ``size``, but are not required for the menu.

All activators must be inside the ``activators`` block in menu root. You can specify one or several activators, or you can skip them. Example:

::

	activators {
	  command: [ // Open menu by one of the command in list
	    "menu",
	    "game"
	  ]
	  join: true // Also open menu on join to server
	}

In this example, the menu will open when you join to the server, or when you enter ``/menu`` or ``/game`` command.

All activators
--------------

.. csv-table::
	:header: "Name", "Data type", "Example", "Description"
	:widths: 5, 5, 10, 30

	"command", |t_list_str|, |ex_below| :ref:`activ-cmd`, "Open when command entered"
	"chat", |t_list_str|, |ex_below| :ref:`activ-chat`, "Open when entered some chat message"
	"containsChat", |t_list_str|, |ex_below| :ref:`activ-chat`, "Open when chat message contains some word"
	"join", |t_bool|, ``join: true``, "Open when player join the server "
	"regionJoin", |t_list_str|, |ex_below| :ref:`activ-wg`, "Open when player joined some WG region"
	"regionLeave", |t_list_str|, |ex_below| :ref:`activ-wg`, "Open when player leaved some WG region"
	"clickItem", |t_list_obj|, |ex_below| :ref:`activ-item`, "Open when RMB click on the item in the hand"
	"clickNPC", |t_list_int|, |ex_below| :ref:`activ-npc`, "Open when RMB click on NPC (from Citizens)"
	"clickEntity", |t_list_obj|, |ex_below| :ref:`activ-entity`, "Open when RMB click on entity"
	"shiftClickEntity", |t_list_obj|, |ex_below| :ref:`activ-entity`, "Open when Shift-RMB click on entity"
	"button", |t_obj|, |ex_below| :ref:`activ-btn`, "Open when button clicked"
	"lever", |t_obj|, |ex_below| :ref:`activ-btn`, "Open when lever shifted"
	"plate", |t_obj|, |ex_below| :ref:`activ-btn`, "Open when plate activated"
	"table", |t_list_str|, |ex_below| :ref:`activ-sign`, "Open when sign with some text clicked"
	"clickBlock", |t_obj|, |ex_below| :ref:`activ-block`, "Open when some block clicked"

.. _activ-cmd:

Commands
--------

This is a :ref:`strings list <hocon-list-str>` with command names. If player enter one of specified commands, menu will be opened. Example:

::

	command: [
	  "menu",
	  "game"
	]

When you use ``/menu`` or ``/game`` command menu will be opened.

If you use the only command, you can use feature of HOCON lists:

::

	command: "menu"

.. _activ-chat:

Chat
----

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

WG regions
----------

These activators can only be used with the `WorldGuard <https://dev.bukkit.org/projects/worldguard>`_ plugin. Also you need to enable ``worldGuard`` parameter in the AbstractMenus general config.

Region enter
~~~~~~~~~~~~

Here listed regions which will open the menu when entering into them. Example:

::

	regionJoin: [  // Open a menu when enter region
	  "spawn",
	  "otherRegion"
	]

In this example, the menu will be opened if you enter to ``spawn`` or ``otherRegion`` region.

Region leave
~~~~~~~~~~~~

Here listed regions which will open the menu when leaving a region. Example:

::

	regionLeave: [  // Open menu when leaving from region
	  "pvp",
	  "someRegion"
	]

In this example, the menu will be opened if you leave from ``spawn`` or ``otherRegion`` region.

.. _activ-item:

Item click
----------

You can add activator to open menu when some item clicked by right click in player's hand.

::

	clickItem {
	  material: STONE
	  name: "Open menu"
	}

.. tip:: Make sure that you specified all item properties. If some property missing, plugin won't handle click on required item.

.. _activ-npc:

Citizens NPC click
------------------

Here listed NPC id which will open the menu when click NPC. Example:

::

	clickNPC: [
	  1,
	  23
	]

In this example, the menu will be opened if you clicked on the NPC with id ``1`` or ``23``.

.. tip:: To find NPC id just type ``/npc sel`` while you looking at NPC. After this enter ``/npc`` command.

.. _activ-entity:

Entity click
------------

The ``clickEntity`` activator is a :ref:`list of objects <hocon-list-obj>`. Each object is a simple entity data. Example:

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

:type: Bukkit type of the entity.
:name: [Optional]. Display name of the entity.

In this example we specified ``PLAYER`` entity and ``ZOMBIE`` entity with ``&eZombie`` name. If player clicked on any player or zombie named ``&eZombie``, menu will be opened. 

.. tip:: All entity types can be found `here <https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/entity/EntityType.html>`_.

.. _activ-btn:

Buttons, levers, plates
-----------------------

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

.. _activ-sign:

Sign
----

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

.. _activ-block:

Block click
-----------

In this code block you can specify location of some world's block. If player click on this block, menu will be opened.

::

	clickBlock{
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