.. include:: ../_includes/datatypes.rst

Menu generator
==============

.. include:: ../_includes/contents.rst

Basics
------

Generated menu can be created in few steps:

#. Specify the catalog of objects. It can contain additional parameters for filtering objects, etc.
#. Create a matrix of cells, into which items from the catalog will be placed according to the template. The matrix should have as many rows as the menu itself.
#. Specify item templates. It is in these items that the necessary placeholders from the catalog are indicated.
#. The generated menu might have several pages. Special actions can be used to navigate between them.

All catalogs are listed in the end of this page. Here we will show you how to create a simple generated menu. We will use the ``PLAYERS`` catalog, that generates a list of all players on the server.

::

	title: "Generated Menu"
	size: 4

	catalog {
	  type: PLAYERS
	}

	matrix {
	  cells: [
	    "_________",
	    "_xxxxxxx_",
	    "_xxxxxxx_",
	    "_________"
	  ]
	  templates {
	    "x" {
	      skullOwner: "%ctg_player_name%"
	      name: "&7Player &e%ctg_player_name%"
	    }
	  }
	}

Here we have created a menu with 4 rows. Once we have specified the ``catalog`` parameter, the plugin will assume that our menu is auto-generated.

:catalog: Catalog configuration. It always has ``type`` parameter. The ``type`` is a name of the catalog. It can also contains other parameters, depends on the catalog type.
:cells: This is a list of strings, represents a matrix of cells. Each line of this list is ​​a menu row. Each symbol in the line represents an inventory slot. The ``_`` character (or any other character that absent in the ``templates`` block) means an empty slot, that is, no item will be added to that slot. The ``x`` character is the tag which we added before. This means that only the item of the template ``x`` and no other item can be added to this inventory slot.
:templates: Binding items to some tag. Each matrix template must have its own tag. A tag is any Latin character from ``a`` to ``z``, or a special character. After we have created the tag, we can bind any item to it. Note you don't need to specify slot for item, since a slot is automatically computes while generating menu.

As a result, with enough players on the server, we will get menu looks like this:

.. figure:: ../_static/gen_players.png
	:align: center

	Example of generated menu

Switch pages
------------

The catalog can contain many objects, so the menu can be paginated when generates. To switch these pages, there is two special actions: ``pageNext`` and ``pagePrev``, which switch to the next or previous page respectively.

To add buttons for pages switching, you need to define a list of static buttons, as we would do in a simple menu. Example:

::

	title: "Generated menu"
	size: 4

	catalog {
	  type: PLAYERS
	}

	matrix {
	  cells: [
	    "_________",
	    "_xxxxxxx_",
	    "_xxxxxxx_",
	    "_________"
	  ]
	  templates {
	    "x" {
	      skullOwner: "%ctg_player_name%"
	      name: "&7Player &e%ctg_player_name%"
	    }
	  }
	}

	items: [
	  {
	    slot: 27
	    material: ARROW
	    name: "&e&l<<"
	    click {
	      pagePrev: 1
	    }
	  },
	  {
	    slot: 35
	    material: ARROW
	    name: "&e&l>>"
	    click {
	      pageNext: 1
	    }
	  }
	]

Here we have specified the static items with actions for switching the pages of the generated menu. These items will be displayed on all pages of the menu (until we specify some rules for their display). Thus, we can specify any static items in which placeholders from the catalog will also work.

Placeholders
------------

The important part of the auto-generated menus is special placeholders. During menu generation, placeholders are generated for each catalog's object that can be used almost anywhere in the menu.

Catalog placeholders look like this:

::

	%ctg_<placeholder>%

Here ``ctg`` is a prefix, short for ``catalog``. A ``<placeholder>`` is a specific placeholder, which usually would be written as ``%<placeholder>%``, between the characters ``%``.

Each catalog has its own placeholders. You can find them under the description of each one in the end of this page. However, there are also common placeholders that exist outside some catalog. They are described below.

.. csv-table::
	:header: "Placeholder", "Type", "Description"
	:widths: 5, 5, 30

	"page", |t_int|, "Current page index"
	"pages", |t_int|, "Total amount of menu pages"
	"page_next", |t_int|, "Next page index"
	"page_prev", |t_int|, "Previous page index"
	"elements", |t_int|, "Total amount of catalog objects"

For example, to use the ``page`` placeholder from this list, you can write ``%ctg_page%``.

Catalogs
--------

A catalog is a dynamic collection of objects of the same type. Each catalog has its own unique name.
The catalog can also have additional parameters that you can specify in the same ``catalog`` block.
Each catalog has its own placeholders. They can be used almost anywhere in the menu, including templates for generation. Below are the default catalogs available in AbstractMenus.

.. tip:: You can create your own catalogs, using an API.

Players
~~~~~~~

Type: ``PLAYERS``

Returns all online players on the server. Unlike other catalogs, it has no placeholders. Instead, you can use any placeholders, but in the format of catalog placeholders, that is, with the ``ctg_`` prefix. During the generation of the menu, the placeholder will be replaced according to the player's object from the catalog, not the player who opened the menu. Example:

::

	catalog {
	  type: PLAYERS
	}

	matrix {
	  cells: [
	    "_________",
	    "_xxxxxxx_",
	    "_xxxxxxx_",
	    "_________",
	  ]
	  templates {
	    "x" {
	      skullOwner: "%ctg_player_name%"
	      name: "Player level is %ctg_player_level%"
	    }
	  }
	}

That is, after the ``ctg_`` prefix, we can write any placeholder (without the ``%`` chars), and this will be replaced in the context of the player from the catalog. 

In this case, we used ``player_name`` placeholder from PlaceholderAPI.

Worlds
~~~~~~

Type: ``WORLDS``

Returns all worlds of the server. Has the following placeholders:

.. csv-table::
	:header: "Placeholder", "Type", "Description"
	:widths: 5, 5, 30

	"world_name", |t_str|, "The name of the world"
	"world_type", |t_str|, "World type (default, flat, etc.)"
	"world_time", |t_int|, "Time in the world"
	"world_difficulty", |t_str|, "Difficulty of the World"
	"world_players", |t_int|, "Number of players in the world"
	"world_entities", |t_int|, "Number of entities in the world"

Entities
~~~~~~~~

Type: ``ENTITIES``

Returns all entities of the player's world. Has an additional parameter to search for entities of a certain type only.

:allowedTypes: List of strings. If this parameter is specified, only entities of the specified types are returned. See all entity types `here <https://hub.spigotmc.org/javadocs/spigot/org/bukkit/entity/EntityType.html>`_

.. csv-table::
	:header: "Placeholder", "Type", "Description"
	:widths: 5, 5, 30

	"entity_name", |t_str|, "Entity name"
	"entity_custom_name", |t_str|, "Entity custom name"
	"entity_location", |t_str|, "Entity location"
	"entity_uuid", |t_str|, "Unique identifier of the entity"
	"entity_world", |t_str|, "The name of the world in which the entity is located"
	"entity_type", |t_str|, "Entity type"

BungeeCord servers
~~~~~~~~~~~~~~~~~~

Type: ``BUNGEE_SERVERS``

Returns all BungeeCord servers. Works only if ``bungeecord: true`` is set in the plugin configuration.

.. csv-table::
	:header: "Placeholder", "Type", "Description"
	:widths: 5, 5, 30

	"server_name", |t_str|, "Server name"
	"server_online", |t_int|, "Amount of players on the server"

Iterator
~~~~~~~~

Type: ``ITERATOR``

Returns a generated list of numbers from ``A`` to ``B``. Useful for generating an exact number of items from a template.

Has three parameters:

:start: Number. Start value (inclusive)
:end: Number. The final value (inclusive)
:desc: [Optional]. Boolean. If true, numbers will be generated in reverse order. That is if ``start`` is 1, and ``end`` is 10, then it will generate ``10, 9, 8, ..., 1``

Standard placeholders (not catalog placeholders) can be used for these parameters. This means that you can use, for example, the value of a variable.

.. csv-table::
	:header: "Placeholder", "Type", "Description"
	:widths: 5, 5, 30

	"index", |t_int|, "Current number"

So far, these are all available catalogs. In general, the catalog system is designed for independent expansion by the user to suit his needs. Despite, we will modify and add new catalogs over time.