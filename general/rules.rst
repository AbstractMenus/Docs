.. include:: ../_includes/datatypes.rst

.. |ex_below| replace:: See example

Rules
=====

.. include:: ../_includes/contents.rst

A rule is a check before performing some actions. The result of this check influences what actions will be performed.

Rules has simple format. Example:

::

	rules {
	  permission: "some.perm"
	}

You can specify one or several rules, similar to activators and actions. Every new rule must be on the new line. For example:

::

	rules {
	  permission: "some.perm"
	  group: "default"
	  money: 3000
	}

In this example, we check a player for permission "some.perm", a group "default" and money amount in 3000. All next actions will be executed only if player matches all of these rules.

All rules
---------

.. csv-table::
	:header: "Name", "Data type", "Example", "Description"
	:widths: 5, 5, 10, 30

	"permission", |t_list_str|, ``permission: "some.perm"``, "Does the player has a permission"
	"world", |t_str|, ``world: "world"``, "Does the player in specified world"
	"gamemode", |t_str|, ``gamemode: SURVIVAL``, "Compare player's gamemode with specified"
	"group", |t_str|, ``group: "default"``, "Does the player a member of **LuckPerms** group"
	"money", |t_int|, ``money: 2``, "Does the player has enough money. **Vault** required"
	"level", |t_int|, ``level: 1``, "Does the player has required level"
	"xp", |t_int|, ``xp: 20``, "Does the player has required XP"
	"health", |t_int|, ``health: 15``, "Does the player has required HP"
	"foodLevel", |t_int|, ``foodLevel: 5``, "Does the player has required food level"
	"chance", |t_int|, ``chance: 50``, "A random chance to complete this rule in percentages"
	"online", |t_int|, ``online: 17``, "Is there required players count on the server"
	"inventoryItems", |t_list_obj|, |ex_below| :ref:`rule-items`, "Does player has a items, specified in the list"
	"heldItem", |t_obj|, |ex_below| :ref:`rule-held`, "Does the item in the player's main hand match the specified item"
	"existVar", |t_obj|, |ex_below| :ref:`rule-var`, "Does "
	"freeSlot", |t_int|, ``freeSlot: -1`` ``freeSlot: 3``, "Checks player inventory for free slot. If number is greater that ``-1`` it will check specified slot. If number lesser than ``0`` it will check inventory for any free slot"
	"freeSlotCount", |t_int|, ``freeSlotCount: 2``, "Checks player inventory for specify free slots count. If player has a same or more free slots it will return true"
	"**WorldGuard**"
	"region", |t_list_str|, ``region: "myregion"``, "Checks player is in one of the specified **WorldGuard** regions"
	"**BungeeCord**"
	"bungeeOnline", |t_obj|, |ex_below| :ref:`rule-bungee`, "Does bungee server has enough players count"
	"bungeeIsOnline", |t_str|, ``bungeeIsOnline: "lobby"``, "Checks the specified **BunegeCord** server is online"
	"**Complex rules**"
	"if", |t_list_obj|, |ex_below| :ref:`rule-if`, "Checks some text and numeric parameters"
	"js", |t_str|, |ex_below| :ref:`rule-js`, "Execute JavaScript script and check the result"

.. _rule-items:

Inventory items
---------------

Rule to check player's inventory for some items. This is an :ref:`objects list <hocon-list-obj>`, where each object is an item. Items has same format as anywhere in AM. Example:

::

	inventoryItems: [
	  {
	    material: CAKE
	    amount: 5
	  },
	  {
	    material: STONE
	    amount: 2
	  }
	]

.. _rule-held:

Held item
---------

Rule to check a held item.

::

	heldItem {
	  material: CAKE
	  amount: 5
	}

.. _rule-var:

Var existing
------------

Rule to check is there exists some variable.

::

	existVar {
	  player: "Peter" // Specify only if variable is personal. Optional parameter
	  name: "name" // Name of the variable. Required parameter.
	}

:name: Variable name.
:player: **[Optional]** Specify if you need to check personal variable.

.. _rule-bungee:

BungeeCord online
-----------------

Rule to check players count on some BungeeCord server. Example:

::

	bungeeOnline {
	  server: "lobby"
	  online: 20
	}

:server: BungeeCord server name.
:online: Required players count.

.. _rule-if:

"If" rule
---------

Rule to compare some data (for example, placeholder from PAPI) with another data. Example:

::

	if {
	  param: "%player_name%"
	  equals: [
	    "Notch",
	    "DeadMouse"
	  ]
	  equalsIgnoreCase: [
	    "notch",
	    "deadmouse"
	  ]
	  contains: [
	    "dead",
	    "tch"
	  ]
	  less: 6
	  more: 2
	}

Parameter ``param`` is required. All other are optional.

:param: The parameter which you compare (``%player_name%`` is a placeholder which replaces to player name who opened menu).
:equals: Check parameter with list of strings. **Case sensitivity**.
:equalsIgnoreCase: Check parameter with list of strings. **Case insensitivity**
:contains: Check parameter to contains one of the specified substring.
:less: Does the parameter less than the specified number. For numeric parameters only.
:more: Does the parameter greater than the specified number For numeric parameters only.

.. _rule-js:

JavaScript
----------

.. important:: JavaScript engine was removed from official JVM since Java 15. To continue using ``js`` rule, plugin loads this engine at server startup. This possible only for Spigot 1.16.5 and higher. If you use Java 15+ with Spigot less than 1.16.5, the ``js`` rule won't work.

The plugin allows you to use JavaScript code. Code inside must always return the result - ``true`` or ``false``. If the code returns something else, the plugin will suppose this is a ``false``. This rule can be used as a replacement for the ``if`` rule because of its greater versatility. But you should understand that interpreting JavaScript will take longer than the ``if`` rule.

The ``js`` rule supports placeholders. This will allow you to compare them, and manipulate them using JavaScript syntax. Example:

::

	rules {
	  js: "'%player_name%'.length < 5"
	}