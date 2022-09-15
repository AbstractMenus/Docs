.. include:: ../_includes/datatypes.rst

Placeholders
============

.. include:: ../_includes/contents.rst

Placeholder is a part of text concluded between ``%`` chars.

The plugin supports placeholders in any parameter of the item, action, rule and in some activators. It doesn’t matter what data type the parameter accepts.

If the parameter accepts a number, and a placeholder inserted instead of it also returns a number, this will work. Therefore, if you use placeholders for numeric types, make sure that it is replaced with a number, otherwise, there will be an error.

There are default built in AM and third-party (PlaceholderAPI) placeholders.

Built-in placeholders
---------------------

.. csv-table::
	:header: "Placeholder", "Type", "Description"
	:widths: 10, 10, 10

	"**Player placeholders**"
	"%player_name%", |t_str|, "Player name"
	"%player_display_name%", |t_str|, "Display name (with colors)"
	"%player_level%", |t_int|, "Player level"
	"%player_xp%", |t_int|, "Player XP"
	"%player_location%", |t_str|, "Player location"
	"%player_world%", |t_str|, "Player world"
	"%player_uuid%", |t_str|, "UUID of player"
	"%player_gm%", |t_str|, "Bukkit's gamemode name"
	"**Server placeholders**"
	"%server_players%", |t_int|, "Amount of players"
	"%server_players_<world>%", |t_int|, "Amount of players in specified world"
	"%server_name%", |t_str|, "Name of server"
	"%server_ip%", |t_str|, "IP address of server"
	"%server_port%", |t_int|, "Server port"
	"%server_max_players%", |t_str|, "Max players slots"
	"%server_version%", |t_str|, "Version of server"
	"**BungeeCord placeholders**"
	"%bungee_online%", |t_int|, "The total count of players on all BungeeCord network"
	"%bungee_players_<server>%", |t_int|, "Count of players on a specific BungeeCord servers"
	"**Special placeholders**"
	"%hanim_:<animation_name>:<unique_id>%", |t_str|, "Returns next frame of the head animation from ``animated_heads.conf`` file"
	"%ctg_page%", |t_int|, "Current page index"
	"%ctg_pages%", |t_int|, "Total amount of menu pages"
	"%ctg_page_next%", |t_int|, "Next page index"
	"%ctg_page_prev%", |t_int|, "Previous page index"
	"%ctg_elements%", |t_int|, "Total amount of catalog objects"
	"%ctg_<extractor placeholder>%", "Depends on Extractor", "Get value by some extractor from Catalog context"
	"%activator_name%", |t_str|, "Name of activator which opened menu"
	"%activator_<extractor placeholder>%", "Depends on Extractor", "Get value by some extractor from Activator context"
	"%placed_<item_extractor_placeholder>%", "Depends on Extractor", "Get info about last placed item"
	"%placed_slot%", |t_int|, "Get slot index in which last item was placed"
	"%taken_<item_extractor_placeholder>%", "Depends on Extractor", "Get info about last taken item"
	"%taken_slot%", |t_int|, "Get slot index from which last item was taken"
	"%changed_<item_extractor_placeholder>%", "Depends on Extractor", "Get info about final item after placing/taking"

If you have installed `PlaceholderAPI <https://www.spigotmc.org/resources/6245/>`_, some of this placeholder won't work and will be replaced by big variety of placeholders from third party plugin instead. **Special placeholders** always works both with PAPI and without it.

Example of using placeholders
-----------------------------

Below is some examples of using placeholders in menu.

In commands
~~~~~~~~~~~

::

	command {
	  console: "give %player_name% minecraft:diamond_sword"
	}


In items
~~~~~~~~

::

	itemAdd {
	  slot: 0
	  material: STONE
	  name: "&e%player_name% stone"
	}


Placeholders for variables
--------------------------

There are more placeholders to discover e.g. :ref:`placeholders for variables <vars-access>` to get a variable value. You can use them whether you've installed PlaceholderAPI or not.

.. _value-extractors:

Value extractors
----------------

.. important:: Value Extractors **are not independent placeholders**, like ``%player_name%``. They can be used only in some context, like Activator's context or inside generated menus. To know how use extractors as placeholders, see :doc:`../advanced/input` article.

Sometimes AbstractMenus may save some context data. 
For example, when player opened menu with ``clickBlock`` activator, plugin saves block on which player clicked. Activator's placeholders allows you to get access to some context data properties via placeholders. This is possible thanks to the Value Extractors.

For each data exists own extractor that can accept limited amount of placeholders. Below described all extractors and placeholder which they can accept.

.. _extractor-block:

Block extractor
~~~~~~~~~~~~~~~

Get data from Minecraft's block.

.. csv-table::
	:header: "Name", "Note"
	:widths: 5, 10

	"block_type", "Block material"
	"block_data", "Block data (MC 1.12-)"
	"block_world", "Block world"
	"block_x", "Position by X axis"
	"block_y", "Position by Y axis"
	"block_z", "Position by Z axis"
	"block_power", "Redstone block power"
	"block_temp", "Temperature of the biome of this block"
	"block_biome", "Name of biome of this block"

.. _extractor-world:

World extractor
~~~~~~~~~~~~~~~

Get data from some world.

.. csv-table::
	:header: "Name", "Note"
	:widths: 5, 10

	"world_name", "Name of the world"
	"world_difficulty", "World difficulty (peaceful, normal, etc.)"
	"world_max_height", "Max height of building"
	"world_pvp", "Is PVP allowed"
	"world_seed", "Seed value of world"
	"world_time", "The relative in-game time of this world. Analogous to hours * 1000"
	"world_type", "World type name (default, flat, etc.)"
	"world_entities", "Amount of entities (include players) in world"
	"world_players", "Amount of players in world"


.. _extractor-entity:

Entity extractor
~~~~~~~~~~~~~~~~

Get data from some entity. If you sure that this entity is Living entity (animal, monster, player, etc.), you can use placeholders for living entities.

.. note:: If you sure, that entity is a Player, you can use any placeholder (PAPI or bundled) instead of placeholders described below. Then this placeholder will be replaced for player which Entity Extractor handles.

.. csv-table::
	:header: "Name", "Note"
	:widths: 5, 10

	"**All entities**"
	"entity_type", "Type name"
	"entity_id", "Numeric id"
	"entity_uuid", "Unique id"
	"entity_name", "General name"
	"entity_custom_name", "Custom name (if exists)"
	"entity_world", "Entity world name"
	"entity_loc_x", "Position by X axis"
	"entity_loc_y", "Position by Y axis"
	"entity_loc_z", "Position by Z axis"
	"entity_facing", "Entity facing (north, east, etc.)"
	"entity_pose", "Entity pose (MC 1.14+)"
	"entity_ticks_lived", "How much entity lives"
	"**Living entity**"
	"entity_last_damage", "Last damage value"
	"entity_no_damage_ticks", "How much entity exists wothout damage"
	"entity_killer", "Last killer name (if exists)"
	"entity_eye_height", "Eye height value"

.. _extractor-item:

Item extractor
~~~~~~~~~~~~~~

Get data from ItemStack.

.. csv-table::
	:header: "Name", "Note"
	:widths: 5, 10

	"item_type", "Item material"
	"item_data", "Item data (MC 1.12-)"
	"item_amount", "Amount in stack"
	"item_max_stack", "Max possible stack size"
	"item_display_name", "Formatted name"
	"item_localized_name", "Localized name"
	"item_model", "Custom model data (MC 1.14+)"
	"item_serialized", "The whole item serialized into base64 string. Can be used with :ref:`serialized <prop-serialized>` item property"

.. _extractor-region:

Region extractor
~~~~~~~~~~~~~~~~

Get data from WorldGuard region.

.. csv-table::
	:header: "Name", "Note"
	:widths: 5, 10

	"region_id", "Region name"
	"region_priority", "Region priproty"
	"region_type", "Region type (cuboid, poly2d, etc.)"
	"region_owners", "List of owners"
	"region_members", "List of members"
	"region_owners_amount", "Amount of owners"
	"region_members_amount", "Amount of members"

.. _extractor-npc:

NPC extractor
~~~~~~~~~~~~~

Get data from Citizens NPC.

.. csv-table::
	:header: "Name", "Note"
	:widths: 5, 10

	"npc_id", "Numeric NPC id"
	"npc_name", "General name"
	"npc_full_name", "Full name"
	"npc_entity_<entity placeholder>", "Get value from NPC's entity by Entity Value extractor"

For example, you need to get NPC's entity type. According to :ref:`Entity extractor format <extractor-entity>`, your placeholders will looks like this:

::

	npc_entity_type

The only difference is an ``npc_`` prefix.

.. _extractor-cmd:

Command extractor
~~~~~~~~~~~~~~~~~

Get data from parsed command (currently used by activators only).

.. csv-table::
	:header: "Name", "Note"
	:widths: 5, 10

	"cmd_name", "Base name of the command"
	"cmd_args", "Amount of arguments"
	"cmd_arg_<argument key>", "Value of parsed argument by key"
	"<argument key>:<placeholder>", "Get value with regular placeholder by player, entered in command"

For example, you specified argument with key ``username`` by some activator. 
Then, to get value which user entered, you need to use placeholder like this:

::

	cmd_arg_username

More about commands building and reading arguments read in :ref:`building-commands` topic.