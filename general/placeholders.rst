.. include:: ../_includes/datatypes.rst

Placeholders
============

.. include:: ../_includes/contents.rst

Placeholder is a part of text concluded between ``%`` chars.

The plugin supports placeholders in any parameter of the item, action, rule, and activator. It doesn’t matter what data type the parameter accepts. 

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

If you have installed `PlaceholderAPI <https://www.spigotmc.org/resources/6245/>`_, some of this placeholder won't work and will be replaced by big variety of placeholders from third party plugin instead.

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