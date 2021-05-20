.. _entities:

Entities
========
Entities represent the concept of game objects.
A player can be an entity, an enemy can be an entity and a camera can also be represented as an entity.

You may look at an entity like a container of components.
You combine certain components together by adding it to an entity.
This way you can construct, for example, the player.

Entity IDs
^^^^^^^^^^
Entities in this engine's implementation are nothing more than 32-bit integers.

Index
"""""
The bottom 24 bits, 0 to 23, represent an index. With this index an entity can be identified as it represents its position in an array, from here on called entity spot.
Components can use this entity index to map components to that particular entity.

Generational Index
""""""""""""""""""
The top 8 bits, 24 to 31, are the generational index.
This is a special value which gets increased everytime an entity spot changes.
So when an entity gets created or destroyed in that spot.

When trying to get a component through an entity id, the generational index is compared.
If the generational index in that entity spot is different than the generational index in the entity id, it indicates the entity does not actually have that component or has been destroyed.

Creating an entity
^^^^^^^^^^^^^^^^^^
Entities need to be created through a level.

.. code-block:: cpp

	zel_entity_id entity = zel_level_create_entity(example_level);

As you can see the function ``zel_level_create_entity`` returns a ``zel_entity_id``.
This is nothing more than a type definition.

.. code-block:: cpp

	typedef uint32_t zel_entity_id;

A 32-bit unsigned integer is used to represent an entity id, as was mentioned above. To learn more about how an entity id gets constructed, please see `zel_ecs.h <https://github.com/NVriezen/ZelGameEngine/blob/master/ZelGameEngine/include/zel_ecs.h#L11>`_.
