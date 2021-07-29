.. include:: ../_includes/datatypes.rst

.. |ex_below| replace:: See example

Actions
=======

.. include:: ../_includes/contents.rst

Action is something that happens after specific event (for example item click or menu opening). The plugin has a lot of actions. Below is a full list of actions.

All actions
-----------

.. csv-table::
	:header: "Name", "Data type", "Example", "Description"
	:widths: 5, 5, 10, 30

	"openMenu", |t_str|, ``openMenu: "my_super_menu"``, "Open a menu with the specified name"
	"closeMenu", |t_bool| or |t_int|, ``closeMenu: true`` ``closeMenu: 30``, "Close current menu. If there is number instead of boolean, menu will be closed after delay in specified ticks"
	"refreshMenu", |t_bool| or |t_int|, ``refreshMenu: true`` ``refreshMenu: 20``, "Update all menu content except the title. If there is number instead of boolean, menu will be updated after delay in specified ticks"
	"message", |t_obj| or |t_str|, |ex_below| :ref:`action-msg`, "Send message to player. There is ability to send simple text, JSON text, title, etc."
	"playerChat", |t_list_str|, ``playerChat: "Hello!"``, "Send a message on behalf of the player who opened the menu"
	"command", |t_obj|, |ex_below| :ref:`action-cmd`, "Execute a list of commands on behalf of a player or server"
	"teleport", |t_obj|, |ex_below| :ref:`action-tp`, "Teleport player to location"
	"itemAdd", |t_list_obj|, |ex_below| :ref:`action-itemadd`, "Add any items to player"
	"itemRemove", |t_list_obj|, |ex_below| :ref:`action-itemrem`, "Remove items from player's inventory. Items will be compared by specified properties or could be just removed by slot number"
	"itemClear", |t_list_obj|, Same as :ref:`action-itemrem`, "Remove item from player's inventory with same way as in ``itemRemove`` action, but independ from itemstack size. So ``count`` property has no effect on this"
	"inventoryClear", |t_bool|, ``inventoryClear: true``, "Fully clear player's inventory"
	"bungeeConnect", |t_str|, ``bungeeConnect: "lobby"``, "Connect player to another BungeeCord server"
	"giveMoney", |t_int|, ``giveMoney: 100``, "Gives money for player. **Vault** required"
	"takeMoney", |t_int|, ``takeMoney: 15.4``, "Takes money from player. **Vault** required"
	"givePermission", |t_str|, ``givePermission: "some.perm"``, "Give permission for player. **LuckPerms** required for correct working"
	"removePermission", |t_str|, ``removePermission: "some.perm"``, "Remove player's permission. **LuckPerms** required for correct working"
	"addGroup", |t_str|, ``addGroup: "vip"``, "Add player to permission group. **LuckPerms** required"
	"removeGroup", |t_str|, ``removeGroup: "admin"``, "Remove player from permission group. **LuckPerms** required"
	"setHealth", |t_int|, ``setHealth: 20``, "Set player health level"
	"setFoodLevel", |t_int|, ``setFoodLevel: 20``, "Set player food level"
	"giveXp", |t_int|, ``giveXp: 1000``, "Give XP points for player"
	"takeXp", |t_int|, ``takeXp: 500``, "Take player's XP points"
	"giveLevel", |t_int|, ``giveLevel: 1``, "Increase level for player on specified value"
	"takeLevel", |t_int|, ``takeLevel: 3``, "Decrease player's level on specified value"
	"sound", |t_obj|, |ex_below| :ref:`action-sound`, "Play sound"
	"potionEffect", |t_list_obj|, |ex_below| :ref:`action-potion`, "Give potion effect to player"
	"removePotionEffect", |t_list_str|, |ex_below| :ref:`action-rempotion`, "Remove potion effect from player"
	"openBook", |t_obj|, |ex_below| :ref:`action-book`, "Create and open a written book for player"
	"setProperty", |t_obj|, |ex_below| :ref:`action-setprop`, "Set new or overwrite existing properties for the menu item"
	"remProperty", |t_list_obj|, |ex_below| :ref:`action-remprop`, "Remove specified properties from the menu item"
	"refreshItem", |t_mult|, |ex_below| :ref:`action-refreshitem`, "Update only one menu item, without updating the entire menu"
	"setSkin", |t_obj|, |ex_below| :ref:`action-setskin`, "Set skin for player using the **SkinsRestorer** plugin"
	"resetSkin", |t_bool|, ``resetSkin: true``, "Reset player’s skin using the **SkinsRestorer** plugin"
	"addRecipe", |t_list_obj|, |ex_below| :ref:`action-recipe`, "Add new custom recipes for crafting"
	"**Variables management**"
	"setVar", |t_list_obj|, |ex_below| :ref:`action-setvar`, "Create or replace variable"
	"removeVar", |t_list_obj|, |ex_below| :ref:`action-remvar`, "Remove variable"
	"incVar", |t_list_obj|, |ex_below| :ref:`action-varmath`, "Increment numeric variable"
	"decVar", |t_list_obj|, |ex_below| :ref:`action-varmath`, "Decrement numeric variable"
	"mulVar", |t_list_obj|, |ex_below| :ref:`action-varmath`, "Multiply numeric variable"
	"divVar", |t_list_obj|, |ex_below| :ref:`action-varmath`, "Divide numeric variable"
	"**Special actions**"
	"delay", |t_obj|, |ex_below| :ref:`action-delay`, "Wrap actions block to perform they after some delay"
	"bulk", |t_list_obj|, |ex_below| :ref:`action-bulk`, "Perform many actions, even of one type"
	"randActions", |t_list_obj|, |ex_below| :ref:`action-randactions`, "Perform random actions block from the list"
	"**For generated menus**"
	"pagePrev", |t_int|, ``pagePrev: 1``, "Switch to one of the previous page. Works only with generated menus"
	"pageNext", |t_int|, ``pageNext: 1``, "Switch to one of the next page. Works only with generated menus"

.. _action-msg:

Message
-------

Action for send various text information to player. For example:

::

	message {
	    chat: [
	        "Line 1",
	        "line 2"
	    ]
	    title: "Title"
	    subtitle: "Subtitle"
	    fadeIn: 10
	    stay: 20
	    fadeOut: 10
	    actionbar: "&aHello"
	    json: "{'text'='Hello'}"
	}

This block has many parameters that can be used separately.

.. csv-table::
	:header: "Name", "Data type", "Description"
	:widths: 5, 5, 30

	"chat", |t_list_str|, "Send personal message to chat"
	"actionbar", |t_str|, "Send text to player's action bar"
	"json", |t_str|, "Send personal JSON message to chat. Works on MC 1.9+"
	"**Title parameters**"
	"title", |t_str|, "Send title"
	"subtitle", |t_str|, "Send subtitle"
	"fadeIn", |t_int|, "Fade in time in ticks"
	"stay", |t_int|, "Stay time in ticks"
	"fadeOut", |t_int|, "Fade out time in ticks"

You can also use the message block as a simple text parameter. Then this action will send chat message to the player.

::

	message: "Single message"

JSON message can be specified not only as a string, but also using the HOCON syntax. Example:

::

	message {
	   json {
	     text: "&aSome text"
	     hoverEvent {
	       action: "show_text"
	       value: "&eSome text"
	     }
	   }
	}

This block is equivalent to this JSON message:

::

	message {
	   json: "{'text':'&aSome text', 'hoverEvent':{'action':100:'show_text', 'value':'&eSome text'}}"
	}

.. tip:: See more information about JSON text `here <https://minecraft.gamepedia.com/Commands#Raw_JSON_text>`_

.. _action-cmd:

Command
-------

Action to execute command on behalf of player or server. Example:

::

	command {
	    player: [
	        "command 1",
	        "command 2"
	    ]
	    console: [
	        "command 1",
	        "command 2"
	    ]
	}

This action will execute two commands. Commands ``/command 1`` and ``/command 2`` will be executed by player and same by server.

You can also execute commands only by player or server:

::

	command { player: "command 1" }

And same with ``console`` block.

.. _action-tp:

Teleport
--------

Teleport player to specified location. Example:

::

	teleport {
	    world: "world"
	    x: 0.0
	    y: 100.0
	    z: 0.0
	    yaw: 0.0
	    pitch: 0.0
	}

This action also has short version:

::

	teleport: "world, 0.0, 100.0, 0.0, 0.0, 0.0"


.. _action-itemadd:

Add item
--------

Add item or items to player's inventory. Item has format described on :doc:`item_format` page. Example:

::

	itemAdd: [
	    {
	        slot: 0
	        material: STONE
	        name: "My stone"
	    },
	    {
	        material: CAKE
	        name: "My cake"
	    }
	]

Items without ``slot`` will be added in the first empty slot in inventory

.. tip:: Don't forget about HOCON :ref:`hocon-list-feature`, where you can specify single list element as just single parameter.

.. _action-itemrem:

Remove item
-----------

This action same as ``itemAdd``. But items will be removed in specified slot. If ``slot`` is not specified, item will be removed by comparing with specified parameters. Example:

::

	itemRemove: [
	    {
	        slot: 0
	        material: STONE
	        name: "My stone"
	    },
	    {
	        material: CAKE
	        name: "My cake"
	    }
	]

.. _action-sound:

Sound
-----

Action to play sound. Below all parameters that can be specified:

::

	sound {
	    name: "SOUND_NAME"
	    volume: 1.0
	    pitch: 1.0
	    public: false
	    location {
	        world: "world"
	        x: 0.0
	        y: 0.0
	        z: 0.0
	    }
	}

Parameter ``name`` required. All parameters are optional.

:name: Bukkit's name of the sound. See all sound names `here <https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/Sound.html>`_
:volume: Volume of the sound. Value between ``0.0`` and ``1.0``.
:pitch: Pitch of the sound. Value between ``0.0`` and ``10.0``.
:public: Is this sound will be played for all near players.
:location: Location in which sound will play. Location format same as in :ref:`action-tp` action.

You can also use short format:

::

	sound: "SOUND_NAME"

In this case, the sound will play only for player in player's location.

.. _action-potion:

Add potion effect
-----------------

Action to add a potion effect to player.

::

	potionEffect: [
	    {
	        effectType: FAST_DIGGING
	        duration: 100
	        amplifier: 1
	    }
	]

The format of this action is similar to :ref:`prop-potion` item property.

.. _action-rempotion:

Remove potion effect
--------------------

Action to remove potion effects from player. Example:

::

	removePotionEffect: [
	    FAST_DIGGING,
	    SPEED
	]

Action above will remove two potion effects from player.

.. _action-book:

Open book
---------

Action to open written book. Format of this action similar to :ref:`prop-book` item property.

::

	openBook {
	  author: "Peter Piper"
	  title: "&e&lSuper Title"
	  pages: [
	    "A content of the first page",
	    "A content of the second page",
	    "..."
	  ]
	}

.. _action-vars:

Variables
---------

.. seealso:: More about variables see on :doc:`variables` page.

.. _action-setvar:

Set
~~~

Action to create new or rewrite exists variable.

::

	setVar {
	  name: "my_var"
	  value: "Some data"
	}

:name: Unique name of the variable.
:value: Value of the variable.
:player: **[Optional]** Use this to create personal variable. Placeholders supported.
:time: **[Optional]** Variable lifetime.
:replace: **[Optional]** If ``false`` and variable with this name exists then it won't be replaced with new value. By default is ``true``.

You can create a temporary variable. A temporary variables automatically removed after specified time. To set a lifetime for the variable you need to add ``time`` parameter.

::

	setVar {
	  name: "%player_name%"
	  name: "my_var"
	  time: "10m"
	  value: "Some data"
	}

In example above we set 10 minutes lifetime for personal variable ``my_var``. You can specify time by seconds, minutes, hours and days. Also you can combine it. For example:

:1d 12h: 1 day and 12 hours (by day we means 24 hours)
:2h 30m: 2 hours and 30 minutes
:10s 10h: 10 hours and 10 seconds

If you want to protect created variable from rewriting, just add ``replace: false`` parameter:

::

	setVar {
	  player: "PeterPiper"
	  name: "my_var"
	  replace: false
	  value: "Some data"
	}

If you want to set several variables at once, you can specify them as an :ref:`objects list <hocon-list-obj>`, where each object is a block, similar to single ``setVar``.

::

	setVar: [
	   {
	     player: "PeterPiper"
	     name: "var1"
	     value: "value1"
	     replace: false
	   },
	   {
	     player: "PeterPiper"
	     name: "var2"
	     value: 58
	   }
	]


.. _action-remvar:

Remove
~~~~~~

Action to remove personal or global variable. Example:

::

	removeVar {
	  name: "myvar"
	  player: "%player_name%"
	}

There is only two parameters:

:name: Name of the variable
:player: **[Optional]** For personal variables

This action also support multiple variables removing:

::

	removeVar: [
	  {
	    name: "myvar"
		player: "%player_name%" 
	  },
	  {
	    name: "my_global_var"
	  }
	]

.. _action-varmath:

Variables math
~~~~~~~~~~~~~~

There is several actions to do math operations with variables:

:incVar: Increment variable on some value.
:decVar: Decrement variable on some value.
:mulVar: Multiply variable by some value.
:divVar: Divide variable by some value.

If variable with specified name doesn't exists it will create a new one with specified value.

Below is the example of using ``incVar`` action. All other math actions has same format.

::

	incVar {
	  name: "my_var"
	  value: 2
	}

Or increment personal variable:

::

	incVar {
	  player: "%player_name%"
	  name: "my_var"
	  value: 2
	}

.. note:: This actions also can change many variables at once. The format is the same as described for ``setVar`` action.

.. _action-delay:

Delay
-----

This action is a kind of wrapper for other actions. It has two parameters:

:delay: Delay in ticks before performing ``actions`` block.
:actions: The actions block, similar to all others. All actions and rules inside will be performed after the time specified in ``delay`` block has expired.

Example for ``click`` block:

::

	click {
	  message: "Hi!"
	  delay {
	    delay: 20
	    actions {
	      itemAdd {
	        material: CAKE
	        name: "&bYour cake"
	      }
	    }
	  }
	  closeMenu: true
	}

In this example, the sequence of execution will be as follows:

#. Sends message "&aHi!".
#. Menu closes.
#. After 20 ticks, a cake will be added to the player’s inventory.

.. _action-bulk:

Bulk
----

Actions wrapper to perform many actions with similar names in a certain sequence. It takes a list of individual actions groups.

::

	bulk: [
	  {
	    message: "Hello!"
	    sound: ENTITY_VILLAGER_NO
	  },
	  {
	    sound: ENTITY_VILLAGER_YES
	  },
	  {
	    sound: BLOCK_NOTE_BLOCK_PLING
	  }
	]

Each element of the list is a separate group of actions, the same as in the usual ``actions`` block. This division into groups allows you to perform similar actions several times.

.. _action-randactions:

Random actions
--------------

Actions wrapper to perform one, randomly selected actions block. When the time comes to execute the ``randActions`` action, this will execute random actions block from list. Example:

::

	randActions: [
	  {
	    message: "Action 1"
	  },
	  {
	    message: "Action 2"
	  },
	  {
	    message: "Action 3"
	  }
	]

After performing this action several times player will see different messages.

.. _action-setskin:

Set skin
--------

To manipulate skins, AbstractMenus uses the `SkinsRestorer <https://www.spigotmc.org/resources/2124/>`_ plugin.

To set the skin to the player, you need to have its **texture** and **signature**. You can get them, for example, by uploading an image to `MineSkin <https://mineskin.org>`_. Next, copy the data from the fields "Texture Data" and "Texture Signature" into the corresponding parameters of ``setSkin`` action.

::

	setSkin {
	  texture: "ewogICJ0aW1lc3RhbXAiIDogMTU4ODQxODUwNjMzOSwKICAicHJvZmlsZUlkIiA6ICJiMGQ0YjI4YmMxZDc0ODg5YWYwZTg2NjFjZWU5NmFhYiIsCiAgInByb2ZpbGVOYW1lIiA6ICJNaW5lU2tpbl9vcmciLAogICJzaWduYXR1cmVSZXF1aXJlZCIgOiB0cnVlLAogICJ0ZXh0dXJlcyIgOiB7CiAgICAiU0tJTiIgOiB7CiAgICAgICJ1cmwiIDogImh0dHA6Ly90ZXh0dXJlcy5taW5lY3JhZnQubmV0L3RleHR1cmUvMmQxNWQxNmE5OTU3NmRiZGE5NWIxMjA3ZmU1NGQ0MjE0Njg4MGMzNzkwNTMwOWViOTg4ODIxMDRhYzcwZjFkNSIKICAgIH0KICB9Cn0="
	  signature: "dHlo+kNagxVCC5CscZrQB5iyQvCBSvG+onsB+cl2qSChe+mILMLWdLi8+stVYo7+X4mjJ9n6F7REW0ndf4fYR72x8xOdOhqgwwtFaA7dyb+NI5AtTQNoch0Tz91FqKWznTWVHBaRQB/eiBpjQz6X1H/dJDAvuN5O6gVLvHcBoBNnQ/ZLVYGrVoK3vOykbEW9NECVCu62bsroVZ0mRMaK35wVP6Wh7srkXGOoyiWuX/gqFf1W/Gpum2SsBx06166Itbu1DNs99ST+Uqx3Vv2THA+CKEpYG7tXPZZ1JSw7Pgk6KzIKyQKmIgL3SphEbzNT6XcUQlpsdzp8q3LECeWLqqk1rQwNqWmduUdsbIiRT4YSwNLyFeClE3NcPGxrPgWXCSfg+u5kHt4+n+u3s469R6DdTGXmtz+Tx06iUzwAgBAd4iNN/rDTranVZ9JokOgrNOQc/uUV4HFxYbrDAh4/LnFP4m/V7HSp5VGZye/Z1yzHtECKZFhId4iTQacckD9HT6vzvT7kH7aM7pkOzG12WkVPYBJBObgXoBUTYlAbCj18DUerT7Fx3m0vi0ThWU/fYmj8XqUFOgmzUFXeq165I0wQhpAv3JiCA0+tVCpndzD6J4MQloBRFHS1gJmI9XYGlK8G24FP8GLljW5RLfHDc672UVzdwI3m4vuFeraLjl4="
	}

.. attention:: Before this action, you need to set the ``closeMenu`` action, because when the skin is changed, player respawning. If the menu is opened, it may cause a critical error for the client.

.. _action-recipe:

Add recipe
----------

Action to add recipe to player. This is a `list of objects <hocon-list-obj>`_ where each object is a recipe.

::

	addRecipe: [
	  {
	    key: "my_recipe"
	    shape: [
	      " A ",
	      "ABA",
	      " A "
	    ]
	    ingredients {
	      A: STONE
	      B: GRASS
	    }
	    result {
	      material: EMERALD
	      name: "Super emerald"
	    }
	  }
	]

There is several required parameters:

:key: Unique id (name) of the recipe.
:shape: Workbench cells. It shows in which shape you want to layout the objects.
:ingredients: Set ingredients in the ``<key>: <material>`` format. ``A`` and ``B`` are those designations from the shape parameter. Only materials supports.
:result: The item, which player receive after crafting.

As a result, in order to craft our item, you need to put the corresponding items in the desired cells, as shown in the picture.

.. figure:: ../_static/actions_recipe.png
	:align: center

	Workbench content

.. _action-setprop:

Set item property
-----------------

Action to add property to the menu item. Example:

::

	items: [
	  {
	    slot: 0
	    material: STONE
	    name: "Properties test"
	    click {
	      setProperty {
	        glow: true
	      }
	    }
	  }
	]

After click the item will glow.

This action can also be used in the block of actions after some rules checking. Since we do not want the result of the rule check to affect the display of the item, we will use the ``mrules`` block, which does not affect this, but simply does an independent rule check. Example:

::

	items: [
	  {
	    slot: 0
	    material: STONE
	    name: "Properties test"
	    mrules {
	      gamemode: CREATIVE
	      actions {
	        setProperty {
	          glow: true
	        }
	      }
	    }
	  }
	]

Now, when you open the menu, the item will glow only if the player has the ``CREATIVE`` mode.

This action also allows you to change the properties of other items in the opened menu. To do this, you need to specify the slot in which it is located. Example:

::

	items: [
	  {
	    slot: 0
	    material: STONE
	    name: "Button 1"
	    click {
	      setProperty {
	        slot: 1 // Slot of the another button
	        glow: true
	        lore: "You've clicked on Button 1"
	      }
	    }
	  },
	  {
	    slot: 1
	    material: STONE
	    name: "Button 2"
	  }
	]


.. _action-remprop:

Remove item property
--------------------

Action to remove the specified properties from the menu item. To do this, you must specify a list of names for these properties. This action, like ``setProperty``, only affects the copy of the menu that the player has opened. It does not change the properties of items globally.

::

	items: [
	  {
	    slot: 0
	    material: STONE
	    name: "Button 1"
	    glow: true
	    click {
	      remProperty: [
	        "glow",
	        "name"
	      ]
	    }
	  },
	]

If you click on this button, only ``material`` property will remain. If you reopen the menu, the deleted properties will return.

You can remove the properties of another item in the opened menu. To do this, the ``remProperty`` block needs to be turned into an object with two elements - the ``slot`` parameter and the list of property ``properties``.

::

	items: [
	  {
	    slot: 0
	    material: STONE
	    name: "Button 1"
	    click {
	      remProperty {
	        slot: 1
	        properties: [
	          "glow",
	          "lore"
	        ]
	      }
	    }
	  },
	  {
	    slot: 1
	    material: STONE
	    name: "Button 2"
	    glow: true
	    lore: [
	      "This is a button 2"
	    ]
	  }
	]

Now when you click on **Button 1** you will remove the ``glow`` and ``lore`` properties of **Button 2**.

.. _action-refreshitem:

Refresh item
------------

Refresh only one item from the whole menu. This is very useful for optimizing your menus because instead of updating the entire menu with all items, you just update one item, which makes it work faster and reduces the server load.

This action has three modes.

Mode 1. Refresh current
~~~~~~~~~~~~~~~~~~~~~~~

Refreshes the current menu item. Example:

::

	items: [
	  {
	    slot: 0
	    material: STONE
	    name: "Xp is: %player_xp%"
	    click {
	      giveXp: 100
	      refreshItem: true
	    }
	  },
	]


Mode 1. Delayed refresh
~~~~~~~~~~~~~~~~~~~~~~~

Refresh the current menu item with a delay specified in ticks.

::

	items: [
	  {
	    slot: 0
	    material: STONE
	    name: "Xp is: %player_xp%"
	    click {
	      giveXp: 100
	      refreshItem: 20 // Ticks instead of 'true'
	    }
	  }
	]


Mode 1. Refresh another item
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Update item in the specified slot. Optionally, you can specify a delay.

::

	items: [
	  {
	    slot: 0
	    material: STONE
	    name: "Button"
	    click {
	      giveXp: 100
	      refreshItem {
	        slot: 1
	        delay: 20 // Optional
	      }
	    }
	  },
	  {
	    slot: 1
	    material: STONE
	    name: "Xp is: %player_xp%"
	  },
	]
