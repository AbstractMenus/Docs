Data Handlers
=============

.. include:: ../_includes/contents.rst

.. note:: In many examples we do not adhere to some Java conventions and common code style to avoid boilerplate code. We want to show how to use API, and not how to write the proper code in Java.

Data handlers is easy way to replace some default logic if plugin. AbstractMenus has several handlers. Interfaces for these handler you can find `here <https://abstractmenus.github.io/api/ru/abstractmenus/api/handler/package-summary.html>`_.

For example, let's override default economy handler. For this we need to implement `EconomyHandler <https://abstractmenus.github.io/api/ru/abstractmenus/api/handler/EconomyHandler.html>`_ interface.

We will create simple implementation which stores balance of each player in the ``Map``.

.. code-block:: java

	public class MyEcoHandler implements EconomyHandler {

	    public final Map<String, Double> balance = new HashMap<>();

	    @Override
	    public boolean hasBalance(Player player, double bal) {
	        return balance.getOrDefault(player.getName(), 0.0) >= bal;
	    }

	    @Override
	    public void takeBalance(Player player, double amount) {
	        double bal = balance.getOrDefault(player.getName(), 0.0);
	        bal -= amount;
	        bal = Math.max(0, bal);
	        balance.put(player.getName(), bal);
	    }

	    @Override
	    public void giveBalance(Player player, double amount) {
	        double bal = balance.getOrDefault(player.getName(), 0.0);
	        bal += amount;
	        balance.put(player.getName(), bal);
	    }
	}

Now we need to register this handler over default. For this we need `Handlers <https://abstractmenus.github.io/api/ru/abstractmenus/api/Handlers.html>`_ manager. It has static methods to register handler or get access to one of them. AbstractMenus also uses these methods.

To register economy handler we need to call ``setEconomyHandler`` method.

.. code-block:: java

	Handlers.setEconomyHandler(new MyEcoHandler());

Thats all. Now AbstractMenus will use this implementation of economy handler, not default.

.. important:: To replace handler, you need to call ``set<...>Handler`` method of ``Handlers`` class inside ``onEnable`` method. Also make sure you added ``AbstractMenus`` plugin as dependency in ``plugin.yml``.