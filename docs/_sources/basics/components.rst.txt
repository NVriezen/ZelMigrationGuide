.. _components:

Components
==========
Components are represented as Plain Old Data or POD structs.
That means components do not contain any logic, they only contain data.
Components are manipulated through :ref:`Systems<systems>`.

Some components are built in to the engine.
To see which components are built in or predefined, please see :ref:`zel_component_builtin<zel_component_builtin_h>`.

Registering components
^^^^^^^^^^^^^^^^^^^^^^
To be able to use components in a level, you first have to register them.
This way you keep the memory footprint to a minimum.
This registering of components can be done at any moment.
But it is recommended to do this while initializing a level.

.. code-block:: cpp

	zel_level_register_component<zel_transform_t>(example_level);

Some components may need to free memory when getting destroyed. For these components you can call a different function to register them.

.. code-block:: cpp

	zel_level_register_component_with_destroy<zel_material_t>(example_level, zel_material_destroy);

As you can see this function takes two parameters. The second parameter is the callback which handles the cleanup of that component. You can see it as the destructor of that component type.

Adding components
^^^^^^^^^^^^^^^^^
Once you registered the component type you want to use, you can add this component to entities.
First create the component itself, for example a ``transform`` component.

.. code-block:: cpp

	zel_transform_t transform{ { 0.0f, 0.0f, 0.0f },{ 0.0f, 0.0f, 0.0f },{ 1.0f, 1.0f, 1.0f } };

Then you can add this component to an entity.

.. code-block:: cpp

	zel_level_add_component(example_level, entity, transform);

See the API Reference section for :ref:`zel_level.h<zel_level_h>` for more information.

Checking & getting components
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Of course sometimes you want to know if a component is on a certain entity.
This is also used in the systems to get a list with entities which contain a combination of certain components.

.. code-block:: cpp

	zel_level_has_components<zel_transform_t, zel_material_t>(example_level, entity);

If you know an entity has the component you need, you can use ``zel_level_get_component`` to get a pointer back to that component.

.. code-block:: cpp

	zel_transform_t* transform_component_ptr = zel_level_get_component<zel_transform_t>(example_level, entity);

The pointer should not be freed. It's valid until the component or the entity gets destroyed.




