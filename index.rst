AbstractMenus Docs
==================

The original version of AbstractMenus you can buy `here <https://www.spigotmc.org/resources/75107>`_ or `here <https://www.mc-market.org/resources/20739/>`_

.. meta::
   :description: Documentation for AbstractMenus plugin
   :keywords: abstract, menus, am, docs, gui, plugin

.. toctree::
  :maxdepth: 1
  :caption: Getting Started
  
  start/how_to
  start/hocon
  start/faq

.. toctree::
  :maxdepth: 1
  :caption: General Features

  general/menu_structure
  general/item_format
  general/activators
  general/actions
  general/rules
  general/variables
  general/text_colors
  general/placeholders
  general/examples

.. toctree::
  :maxdepth: 1
  :caption: Advanced Features

  advanced/logical
  advanced/templates
  advanced/animations
  advanced/generation

.. toctree::
  :maxdepth: 1
  :caption: For Developers

  dev/api

General
-------

**Abstract Menus** - it's a Spigot plugin, with the one you can create simple or complex interactive menus. The main difference between this plugin and other similar ones in large opportunities for customization and menu optimization and a large number of tools for creating complex GUI. This plugin can easily replace most plugins for GUI creation, as well as some other simple plugins with GUI only.

Basic concepts
--------------

:Menu file: A file inside menus folder in AbstractMenus plugin folder. In menu file you could describe single or several menus.

:Data type: One of the ways to describe some data via HOCON syntax.

:Activator: An event that caused menu opening.

:Rule: A condition. For example, whether a player has permission, money, etc.

:Action: An action that can be executed. For example, action ``givePermission`` gives some permission to the player.

:Item: An inventory item with some properties. This may be menu button, or object for some checks.

:Placeholder: The part of the text enclosed between ``%``, which is replaced to some data, for example, to the player nickname.

:Variable: Any text or numeric value, saved in plugin's database with the opportunity to change or use it via placeholders.

:Template: Any block or parameter in a file that can be inserted anywhere in the menu file. Templates exist primarily to exclude the copying of entire blocks, such as items, and for convenient menu editing in the future.

:Animation frame: Animation unit, that contains frame-specified items and other useful parameters.

Commands
--------

``/am open <menu_name>``
  Open menu without activators.
``/am open <menu_name> <player>``
  Open menu for some player without activators.
``/am reload``
  Reload every menu from menus folder. For full plugin reloading we advice you reload server.
``/var get <name>``
  Display value of global variable.
``/var set <name> <value>``
  Create or change global variable.
``/var set <name> <value> <true/false replace protection>``
  Create or change global variable with change protection.
``/var set <name> <value> <time>``
  Create or change temporary global variable.
``/var set <name> <value> <time> <true/false replace protection>``
  Create or change temporary global variable with change protection.
``/var rem <name>``
  Remove global variable.
``/var inc <name> <number>``
  Increment numeric global variable.
``/var dec <name> <number>``
  Decrement numeric global variable.
``/var mul <name> <number>``
  Multiply numeric global variable.
``/var div <name> <number>``
  Divide numeric global variable.
``/varp get <player> <name>``
  Display value of personal variable.
``/varp set <player> <name> <value>``
  Create or change personal variable.
``/varp set <player> <name> <value> <true/false replace protection>``
  Create or change personal variable with change protection.
``/varp set <player> <name> <value> <time>``
  Create or change temporary personal variable.
``/varp set <player> <name> <value> <time> <true/false replace protection>``
  Create or change temporary personal variable with change protection.
``/varp rem <player> <name>``
  Remove personal variable.
``/varp inc <player> <name> <number>``
  Increment numeric personal variable.
``/varp dec <player> <name> <number>``
  Decrement numeric personal variable.
``/varp mul <player> <name> <number>``
  Multiply numeric personal variable.
``/varp div <player> <name> <number>``
  Divide numeric personal variable.

Permissions
-----------

``am.admin`` - Allow use all commands, described above.

.. _external-plugins-support:

External plugins support
------------------------

* `Vault <https://www.spigotmc.org/resources/34315/>`_ - Support of any Economy plugin on server. Required for actions and rules to manipulate with player's balance.
* `PlaceholderAPI <https://www.spigotmc.org/resources/6245/>`_ - Many placeholders instead of defaults.
* `LuckPerms <https://www.spigotmc.org/resources/28140/>`_ - For all actions and rules, which use permissions and groups.
* `WorldGuard <https://dev.bukkit.org/projects/worldguard>`_ - Required for activators, which use WG regions.
* `Citizens <https://dev.bukkit.org/projects/citizens>`_ - Required for correct NPC activators working.
* `HeadDatabase <https://www.spigotmc.org/resources/14280/>`_ - Allows you to get any custom head from large database as an item.
* `MMOItems <https://www.spigotmc.org/resources/60876/>`_ - You can get any item from this plugin.
* `SkinsRestorer <https://www.spigotmc.org/resources/2124/>`_ - For actions to change/reset a player's skin.
* `ItemsAdder <https://www.spigotmc.org/resources/73355/>`_ - Takes a custom item stack, defined in plugin's registry by their namespaced id
* `Oraxen <https://www.spigotmc.org/resources/73355/>`_ - Takes a custom item stack, defined by Oraxen plugin