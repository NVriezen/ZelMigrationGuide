.. _levels:

Levels
======
In the Zel Game Engine we use the word **level** to indicate a collection of entities. You can use these levels to create different menus or stages for your game.

Creating a level
^^^^^^^^^^^^^^^^
To create a level you can use the function ``zel_level_create``.

.. code-block:: cpp

	zel_level_t* example_level = zel_level_create("Example Level");

As you can see it returns a pointer to a struct called ``zel_level_t``.
For more information on ``zel_level_t`` see :ref:`here<zel_level_t>`.

.. warning::

	A level manager has not been implemented yet. So currently you need to set your level as the `active_level` in order to make the engine work.

Registering components
^^^^^^^^^^^^^^^^^^^^^^
Before you can use any components you need to register them.
This is to keep the data footprint as low as possible.
So to be able to use the transform component in the level, you can do the following.

.. code-block:: cpp

	zel_level_register_component<zel_transform_t>(example_level);

The recommended way is to register all components immediately after you created the level, but it's possible to register components at any time.

Some components need to clean up some stuff. For example; materials may need to destroy a shader or textures they've used. There is another function enabling that kind of behaviour.

.. code-block:: cpp

	zel_level_register_component_with_destroy<zel_material_t>(example_level, zel_material_destroy);

.. note::

	For more information about which components are built into the engine, please take a look at the :ref:`Components<components>` section.


Registering Systems
^^^^^^^^^^^^^^^^^^^
Systems also need to be registered to a level.
This way you can determine which systems actually get used in a level.

.. code-block:: cpp

	zel_level_register_system(example_level, example_system_update, example_system_name);

The function for registering needs the level of course, but also the system's update function and name. The name can be used to unregister the system again.

Creating entities
^^^^^^^^^^^^^^^^^
Components can't exist on their own.
They need to be attached to an entity.
You can create an entity with ``zel_level_create_entity``.

.. code-block:: cpp

	zel_entity_id entity = zel_level_create_entity(example_level);

This returns a ``zel_entity_id`` which identifies the entity inside the level.

See the sections :ref:`Entities<entities>`, :ref:`Components<components>` and :ref:`Systems<systems>` for more information.