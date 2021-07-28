Variables
=========

.. include:: ../_includes/contents.rst

Variable in AbstractMenus is any data stored in memory and can be accessed through placeholders.

Plugin allows you to save and edit variables through actions or admin commands. Variables can be **global** or **personal**.

:Global variable: Variables accesible from any place.
:Personal variable: Variable binded to player. Variable with same name may has different values for different players.

Variables naming
----------------

You should only use latin symbols and ``_`` when you naming variables, because using any other symbols may call an errors when using in placeholder.

Variable operations
-------------------

You can create, edit, remove and do math operations(if variable is integer). These operations described in details :ref:`here <action-vars>`.

Temporal variables
------------------

By default, variable exists infinite time, while not be deleted by some action. But you can also create a temporal variables. Temporal variables will be deleted automatically after some time. Read :ref:`this <action-setvar>` topic to know how to create temporal variables.

Also you can get a remain lifetime of the variable. For this you have to use special placeholders, more information aboit this in table below.

.. _vars-access:

Access to variables
-------------------

To get value of variable you need to use a :doc:`../general/placeholders`. You can use it even without PlaceholderAPI, but we recommend you to use it. The list of variable placeholders :ref:`below <vars-pls-table>`.

:< >: Required argument.
:[ ]: Optional argument.

.. _vars-pls-table:

.. csv-table::
	:header: "Placeholder", "Description", "Example"
	:widths: 10, 10, 10

	"``%var_:<variable_name>[:<default_value>]%``", "Get global variable value", ``%var_:myvar%`` ``%var_:myvar:No%``
	"``%varp_:<variable_name>[:<default_value>]%``", "Get personal variable value of player who opened menu", ``%varp_:myvar%`` ``%varp_:myvar:No%``
	"``%var_:<player>.<variable_name>[:<default_value>]%``", "Get personal variable value by player name", ``%var_:Notch.myvar%`` ``%var_:Notch.myvar:No%``
	"``%vart_:<variable_name>%``", "Get the lifetime of the global temporal variable. If variable is not temporal it will return 0 seconds", ``%vart_:myvar%`` ``%vart_:another_var%``
	"``%varpt_:<variable_name>%``", "Get the lifetime of the personal temporal variable. If variable is not temporal it will return 0 seconds", ``%varpt_:myvar%`` ``%varpt_:another_var%``

Default value is a text or number on which placeholder replaces if requested variable doesn't exists. If variable doesn't exists and default value didn't set, placeholder will be replaced with empty string.

Synchronizing variables
-----------------------

If you have a bunch of servers and BungeeCord, all variables can be synchronized between the servers on which the plugin is installed. By default this is disabled. But if you need it, you can enable this feature in the plugin configuration. To do this, just set the ``syncVariables`` and ``bungeeCord`` parameters to ``true``.