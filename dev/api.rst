Developers API
==============

.. include:: ../_includes/contents.rst

Add API to the project
----------------------

If you are not using any build system, just add AbtractMenus jar file in the project in your IDE.
If you are using a build system, you can use this artifacts:

Gradle
~~~~~~

.. code-block:: groovy

	repositories {
	    maven { url 'https://jitpack.io' }
	}

	dependencies {
	    compileOnly 'com.github.AbstractMenus:api:main-SNAPSHOT'
	}

Maven
~~~~~

.. code-block:: xml

	<repositories>
	    <repository>
	        <id>jitpack</id>
	        <url>https://jitpack.io</url>
	    </repository>
	</repositories>

	<dependencies>
	    <dependency>
	        <groupId>com.github.AbstractMenus</groupId>
	        <artifactId>api</artifactId>
	        <version>main-SNAPSHOT</version>
	    </dependency>
	</dependencies>

You also need to add ``AbstractMenus`` dependency in ``depend`` list of your ``plugin.yml`` file:

.. code-block:: yaml

	depend:
	  - "AbstractMenus"


TypeToken class and serialization
---------------------------------

To get data from the menu file, you need to use serializer classes. The serializer receives a ``ConfigurationNode`` object (a block from a config file) at the input and returns an object of a certain class.

Serializer is a class that implements the ``TypeSerializer`` interface. It has only two methods for deserialize and serialize objects. To register our data in the plugin, you only need the ``deserialize`` method, since there is no writing operation to the menu file in the plugin.

.. code-block:: java

	public class MySerializer implements TypeSerializer<MyClasss> {

	    @Override
	    public MyClasss deserialize(TypeToken<?> type, ConfigurationNode node) throws ObjectMappingException {
	        // TODO deserialize node to MyClass object
	    }

	    @Override
	    public void serialize(TypeToken<?> type, MyClasss obj, ConfigurationNode value) throws ObjectMappingException {
	        // Not used
	    }
	}

To learn more about using serializers in the HOCON library, which is used in the plugin, you can follow the link: https://docs.spongepowered.org/stable/en/plugin/configuration/serialization.html

To associate the serializer with the data type, we use ``TypeToken`` class. To get an instance of ``TypeToken`` which will represent your data type, just call the static method ``of``.

.. code-block:: java
	
	TypeToken.of(String.class); // TypeToken for the String class

Data registration
-----------------

All data for the plugin must be registered in the ``onLoad()`` method. You need to use this method because all data must be registered before loading the menu, which occurs at the stage of calling the ``onEnable()`` method.

The ``Tokens`` class is used to register data. In order to add your activator, action, etc. it is necessary to create:

#. A class that inherits or implements one of the classes described below.
#. Serializer for this class
#. TypeToken of this class

After you have created all this, it remains to call the corresponding static method of the ``Tokens`` class to register the data.

Create Action
-------------

Every action classe must implement the ``ru.nanit.abstractmenus.api.Action`` interface.

.. code-block:: java

	public class MyAction implements Action {
	    @Override
	    public void activate(Player player, Menu menu, Item clickedItem) {
	        player.sendMessage("Hello! This is my action!");
	    }
	}

This is the simplest example. Note that ``clickedItem`` can be ``null``. It depends on what triggered the action call - a click on an item, or something else.

As mentioned earlier, each action must have its own serializer. Let's add it.

.. code-block:: java

	public class MyAction implements Action {

	    private final String text;

	    private MyAction(String text) {
	        this.text = text;
	    }

	    @Override
	    public void activate(Player player, Menu menu, Item clickedItem) {
	        player.sendMessage(text);
	    }

	    public static class Serializer implements TypeSerializer<MyAction> {

	        @Override
	        public MyAction deserialize(TypeToken<?> type, ConfigurationNode value) {
	            return new MyAction(value.getString ());
	        }

	        @Override
	        public void serialize(TypeToken<?> type, MyAction obj, ConfigurationNode value) {

	        }
	    }
	}

Now our action can take a string parameter and send it in a message to the player. To register our action, create a ``TypeToken`` for it and give it a key (unique name).

.. code-block:: java

	Tokens.registerAction("myAction", TypeToken.of(MyAction.class), new MyAction.Serializer());

Here ``myAction`` is the key of our action. It will be used in the menu file. This is how it might be used in a menu file:

::

	items: [
	  {
	    slot: 1
	    material: STONE
	    name: "My item"
	    click {
	      myAction: "Hello! This is my action!"
	    }
	  }
	]


Create rule
-----------

Every rule implements the ``ru.nanit.abstractmenus.api.Rule interface``. It contains one method, which, depending on the check, can return ``true`` (the player matches the rule) or ``false`` (the player doesn't matches the rule).

.. code-block:: java

	public class MyRule implements Rule {
	    @Override
	    public boolean check(Player player, Menu menu, Item clickedItem) {
	        return player.getName().equals("Bob");
	    }
	}

In this case, the rule will return ``false`` if player's name is not ``Bob``. The creation of a serializer is exactly the same.

.. code-block:: java

	public class MyRule implements Rule {

	    @Override
	    public boolean check(Player player, Menu menu, Item clickedItem) {
	        return player.getName().equals("Bob");
	    }

	    public static class Serializer implements TypeSerializer<MyRule> {

	        @Override
	        public MyRule deserialize(TypeToken<?> type, ConfigurationNode value) {
	            return new MyRule();
	        }

	        @Override
	        public void serialize(TypeToken<?> type, MyRule obj, ConfigurationNode value) {

	        }
	    }
	}

Here we decided not to avoid parameters. Our rule is to still check the player's nickname.

Registration is similar to an action's.

.. code-block:: java

	Tokens.registerRule("myRule", TypeToken.of(MyRule.class), new MyRule.Serializer());

Since our rule does not accept any parameters, when specifying it in the menu file, we can simply specify ``true``.

::

	rules {
	  myRule: true
	}

This applies to all registered actions, rules, etc., which have no parameters.

Create activator
----------------

Each activator inherits from the abstract class ``ru.nanit.abstractmenus.api.Activator``. It has no abstract methods to implement. This class is implemented from the Bukkit's ``Listener`` interface, so inside you can listen to events, the calling of which opens the menu.

.. important:: Do not register activator as listener manually. The plugin will do it automatically.

.. code-block:: java

	public class MyActivator extends Activator {

	    @EventHandler
	    public void onSneak(PlayerToggleSneakEvent event) {
	        if (event.isSneaking()) {
	            openMenu(event.getPlayer());
	        }
	    }

	    public static class Serializer implements TypeSerializer<MyActivator> {

	        @Override
	        public MyActivator deserialize(TypeToken <?> type, ConfigurationNode value) {
	            return new MyActivator();
	        }

	        @Override
	        public void serialize(TypeToken<?> type, MyActivator obj, ConfigurationNode value) {

	        }
	    }
	}

The ``openMenu`` method is a method of the ``Activator`` class. It opens the menu in which this activator is located. In this case, we open the menu if the player toggles sneak on.

.. code-block:: java

	Tokens.registerActivator("myActivator", TypeToken.of(MyActivator.class), new MyActivator.Serializer());

Create catalog
--------------

A catalog is a collection of objects. Each object of the collection has its own placeholders, in order to add them when generating a menu from a template. To create a catalog, we need to inherit two classes - ``Catalog`` and ``Snapshot``.

The ``Snapshot`` class represents a collection of objects already received. An object of this class is created each time the menu is opened.

Each catalog should also have its own serializer. Optionally, you can take additional parameters and pass them to your ``Snapshot`` instance. In our example, we won't do this.

.. code-block:: java

	public class MyCatalog extends Catalog {

	    @Override
	    public Snapshot<?> snapshot(Player player) {
	        return new CatalogSnapshot(player);
	    }

	    public static class CatalogSnapshot extends Snapshot<MyObject> {

	        public CatalogSnapshot(Player player) {
	            super(player);
	        }

	        @Override
	        protected Collection<MyObject> parseElements() {
	            // TODO return some collection of your objects
	        }

	        @Override
	        protected void parsePlaceholders(MyObject element, Map<String, String> map) {
	            map.put("my_placeholder", element.getValue());
	        }

	    }

	    public static class Serializer implements TypeSerializer<MyCatalog> {

	        @Override
	        public MyCatalog deserialize(TypeToken<?> type, ConfigurationNode value) {
	            return new MyCatalog();
	        }

	        @Override
	        public void serialize(TypeToken<?> type, MyCatalog obj, ConfigurationNode value) {

	        }
	    }
	}

In the ``parseElements`` method, we simply return the collection from our objects.

In the ``parsePlaceholders`` method, we can specify placeholders, which will be replaced with the specified values ​​during the menu generation. This method is passed an instance of the object and ``Map`` for adding placeholders.

Registration is the same as always.

.. code-block:: java

	Tokens.registerCatalog("my_catalog", TypeToken.of(MyCatalog.class), new MyCatalog.Serializer());

Now we can use our catalog when generating the menu^

.. code-block:: java

	title: "Menu"
	size: 4

	catalog {
	  type: my_catalog
	} 

	matrix {
	  cells: [
	    "_x_x_x_x_",
	    "_x_x_x_x_",
	    "_x_x_x_x_"
	  ]
	  templates {
	    "x" {
	      material: STONE
	      name: "My placeholder: %ctg_my_placeholder%" // Display your placeholder
	    }
	  }
	}


Create item property
--------------------

An item property is a parameter that determines its appearance and metadata.

To create an item, two types of properties are used:

:Regular property: This property works with already exists ``ItemMeta`` and just modify it. Optionally, regular property can return new ``ItemStack``.
:Material relacer: These are properties that in the process of work assign their material to an object. This makes it possible not to indicate the material in the properties of the object when the material is known from the context. Such a property, for example, is the ``skullOwner`` property, which immediately assigns the ``PLAYER_SKULL`` material to the item.

An example with a regular property. Here we take a regular string and assign it as a name. Note that you can return an item. If the method returns an item, then in the process of building the final item, the changed meta parameter will not be taken. Instead, the item will be replaced with the item you returned. This is useful for dynamically changing an item, including its material, depending on the parameters received.

If ``Optional.EMPTY`` is returned, the item keeps the same, and the modified metadata is assigned to it.

.. code-block:: java

	public class MyProperty implements SimpleProperty {

	    private final String name;
	 
	    private MyProperty (String name) {
	        this.name = name;

	    }

	    @Override
	    public Optional<ItemStack> apply(ItemStack itemStack, ItemMeta meta, Player player, Menu menu) {
	        meta.setDisplayName(name);
	        return Optional.empty();
	    }

	    public static class Serializer implements TypeSerializer<MyProperty> {

	        @Override
	        public MyProperty deserialize(TypeToken<?> type, ConfigurationNode value) {
	            return new MyProperty(value.getString());

	        }

	        @Override
	        public void serialize(TypeToken<?> type, MyProperty obj, ConfigurationNode value) {

	        }
	    }
	}

Example with a material replacer. Here we give out the creeper head. This property has no parameters. Therefore, when using this property, you can simply specify ``true`` when using this property.

.. code-block:: java

	public class MyMaterialProperty implements MaterialProperty {

	    @Override
	    public ItemStack getItem(Player player, Menu menu) {
	        return new ItemStack(Material.CREEPER_HEAD);
	    } 

	    public static class Serializer implements TypeSerializer<MyMaterialProperty> {

	        @Override
	        public MyMaterialProperty deserialize (TypeToken<?> type, ConfigurationNode value) {
	            return new MyMaterialProperty();
	        }

	        @Override
	        public void serialize(TypeToken<?> type, MyMaterialProperty obj, ConfigurationNode value) {

	        }
	    }
	}

Registration properties is no different from what has already been shown above.

.. code-block:: java

	Tokens.registerItemProperty("myProperty", TypeToken.of(MyProperty.class), new MyProperty.Serializer());
	Tokens.registerItemProperty("myMaterialProperty", TypeToken.of(MyMaterialProperty.class), new MyMaterialProperty.Serializer());


Utils
-----

The API contains various utilities for replacing colors, placeholders, etc.

Replace color codes
~~~~~~~~~~~~~~~~~~~

To replace color codes in a string or list of strings, use the ``Colors`` util.

.. code-block:: java

	String str = Colors.of("&aHello!");

Replace placeholders
~~~~~~~~~~~~~~~~~~~~

To replace placeholders in some string, use one of the handlers that can be obtained using the ``Handlers`` class. The placeholder handler works with both default placeholders and PAPI.

.. code-block:: java

	String str = Handlers.getPlaceholderHandler().replace(player, menu, str);