Logical structures
==================

.. include:: ../_includes/contents.rst

In general, all logical structures in AbstractMenus works as **if -> then -> else**. these strustures can be nested.

Actions block
-------------

Each actions block is a complex object that can contains rules and other actions. Below is a real actions block structure:

.. csv-table::
	:header: "Param", "Type", "Destination"
	:widths: 10, 10, 10

	"rules", "Rules block", "Regular rules"
	"actions", "Actions block", "Actions that performs if player matches all rules"
	"denyActions", "Actions block", "Actions that performs if player **doesn't matches** all rules"

You can imagine this block as infinite tree where enery branch is a rule or actions block.

.. figure:: ../_static/logical_actions.jpg
	:align: center

	Actions block structure

Below is an example of using actions block with more complex structure:

::

	title: "Example"
	size: 6
	openActions {
	  message: "Opening the menu..."
	  rules {
	    permission: "super.admin"
	  }
	  actions {
	    message: "You have super.admin permission!"
	    rules {
	      money: 1000
	    }
	    actions {
	      takeMoney: 1000
	    }
	    denyActions {
	      giveMoney: 1000
	    }
	  }
	}

Here is an order of all actions that will be performed when player opens the menu:

#. The message "Opening the menu..." will be displayed in chat.
#. The permission check will run and check if a player has the "super.admin" permission.
#. If he does, than goes to the action block and send message "You have super.admin permission!".
#. Check is player has 1000 money on his balance.
#. If he does, withdraw them.
#. If he doesn't, then add 1000 money to his balance.

.. note:: The rules inside the actions block do not affect on the parent actions blocks. In case of instance above, checking for the permission and availability of money will not affect on menu opening.

Rules block
-----------


Local actions
~~~~~~~~~~~~~

In each rules block you can specofy locl actions block. This useful in case when you cannot use ``actions`` and ``denyActions`` blocks outside. Example:

::

	items: [
	  {
	    slot: 0
	    material: STONE
	    rules {
	      permission: "am.admin"
	      actions {
	        message: "Yes!"
	      }
	      denyActions {
	        message: "Nope"
	      }
	    }
	  }
	]

As always, actions described in the ``actions`` block will be executeds if the player matches all rules in the ``rules`` block. And ``denyActions`` block will be executed if the player doesn't matches at least one of the rules in the current scope.

In the example above, the message "Nope" will send to the player if the player doesn't have the ``am.admin`` permission. The ``actions`` block, if it specified, will work oppositely, that is, if the player matches the rules.

List of rule blocks
~~~~~~~~~~~~~~~~~~~

Actually, any rules block is a :ref:`list of objects <hocon-list-obj>`, where each object is a rules block. Before that, always when we described the rules we just opened the ``rules`` block and wrote the rules there like this:

::

	rules {
	  permission: "super.admin"
	  group: "vip"
	}

Since rules block is a list of other rules block, you can specify similar rules and add own local actions to every block.

::

	click {
	  message: "You clicked on a pebble"
	  rules: [
	    {
	      permission: "my.perm"
	      actions {
	        message: "You have permission"
	      }
	    },
	    {
	      money: 500
	      actions {
	        message: "You have enough money"
	      }
	      denyActions {
	        message: "You don't have enough money. Take it though."
	        giveMoney: 500
	      }
	    }
	  ]
	  actions {
	    message: "You have enough money and the right permission!"
	  }
	}

In this example we used the rules block as a list. Consider in order what happens when you click on an item:

#. The message "You clicked on a pebble."
#. The player will be checked for "my.perm" permission.
#. If there is a permission, the message "You have the right permission" will be displayed.
#. Player will be checked for 500 money on balance.
#. If there is money, the message "You have enough money" will be displayed.
#. If there is no money, the player will receive the message "You don't have enough money and 500 coins will be given.
#. If both checks on the permission and money amount were successful, the message "You have enough money and the right permission!" will be displayed.

.. note:: When using rules as a list, each next element of the list performed independ of the result of checking the previous one. In example above, even if the player doesn't have ``my.perm`` permission, the check for the amount of money will still be performed. But in a global sense, the entire ``rules`` block will no longer be considered successful. So, the message "The player has the right and money!" will not be displayed if at least one of the rules block was not successful.