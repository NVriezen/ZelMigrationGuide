.. _systems:

Systems
=======
Systems contain most of the logic. They process and manipulate the components.
That means you only have input through a combination of components. These components are obtained through entities.
Systems are in essence nothing more than a function.

Template
^^^^^^^^
Systems always need to have the same parameters and a name.
A template is included to make it easier to create systems.
In the ``Application`` project's ``include`` folder you will find ``example_system.h``.
Copy this code to create your own systems.

Registering a system
^^^^^^^^^^^^^^^^^^^^
Systems needs to be registered just like components.
This way you can turn systems on and off when you like.
To register a system you also need to input a name, which is used to unregister a system.

.. code-block:: cpp

	zel_level_register_system(example_level, example_system_update, "example_system");

Unregistering a system is done in somewhat the same way.

.. code-block:: cpp

	zel_level_unregister_system(example_level, "example_system");

For more information, see the :ref:`zel_level.h<zel_level_h>` section.

Entities list iterator
^^^^^^^^^^^^^^^^^^^^^^
You can get a list of entities through the ``zel_entities_list`` iterator.
This iterator goes through all entities and only returns the ones which have the components you need.

.. code-block:: cpp

	zel_entities_list<zel_transform_t> transform_list = zel_entities_list<zel_transform_t>(level);

Then to get the beginning or end of the list.

.. code-block:: cpp

	zel_entities_list<zel_transform_t>::iterator beginning_of_list = transform_list.begin();
	zel_entities_list<zel_transform_t>::iterator end_of_list = transform_list.end();

There is also an easy way to iterate over the entities in the list.
By using a for loop like so:

.. code-block:: cpp

	for (zel_entity_id entity : zel_entities_list<zel_transform_t>(level))
	{
		//use the transform component here
	}

This is the recommended way to use the ``zel_entities_list``.
It's also not restricted to only one type. You can get a list of entities which have a combination of multiple components.

.. code-block:: cpp

	for (zel_entity_id entity : zel_entities_list<zel_transform_t, zel_material_t, zel_mesh_t>(level))
	{
		//use the transform component here
	}


	


