.. _unity_components:

Components
==========

Components in the Zel Game Engine are somewhat similar to the components in Unity.
In Unity you add **Components** to **GameObjects**, while in the Zel Game Engine you add **Components** to **Entities**.

However the biggest difference between the two is that **Components in the Zel Game Engine only contain data**. Nothing else.
The reason for this is to be able to pack this **data closely together** and manipulate this data all at once.

Only Data
---------
Take for example in-game movement. You could have a player and 5 enemies.
They all move in the same way so their **code is identical**.

In Unity you make a base component or class. Let's call it the movement component.
You would write your movement code in there.
The player and enemy classes will **inherit** from this movement class.
So when you call player.Move() or enemy.Move() **the same code gets executed**.

The way the Zel Game Engine handles this is a bit different.
**Data and logic are separated**, but we still have a movement component.
This component only contains the variables for the velocity, acceleration, etc.
We add this component to the player and enemy Entities, just like we would in Unity.

The movement code however, which is identical between player and enemy, is located in a :ref:`System<unity_update>`.
This System **manipulates all movement components**.

.. admonition:: Why?
    :class: in-depth

    The benefit of this is that data is closely packed together in memory.
    When the CPU runs the code it doesn't have to constantly wait for data to be fetched from RAM. Which is a **very slow process** compared to fetching from cache.

    However, the code in the systems, so the **CPU instructions**, also don't have to be fetched that often from RAM.
    Since we use the same system to manipulate the data, the CPU instructions are **already in cache**.
    They **don't change** and therefore the CPU only has to change which data is getting manipulated.

    When using the object oriented approach, you would constantly need to load new data.
    The code may be the same, but when switching from one enemy to the other, the **whole object** gets loaded in cache.
    So if the enemy stores health and attack information for example.
    That would get loaded into the cache, **wasting valuable space**.
    
    You only need the movement data.
    That's exactly why in the data oriented approach you will **split up the data**.
    Reducing the amount of wasted space in cache and therefore the amount of times you load data from RAM.

    For a **quick explanation** what cache is and the difference between cache and main memory(RAM) you can watch `this YouTube video <https://www.youtube.com/watch?v=IA8au8Qr3lo>`_ from Eye on Tech.

    
Using & Adding Components
-------------------------
Adding Components to Entities is somewhat done in the same way as adding Components to GameObjects in Unity.

.. code-block:: cpp

    zel_transform_t transform{ { 0.0f, 0.0f, 0.0f },{ 0.0f, 0.0f, 0.0f },{ 1.0f, 1.0f, 1.0f } };
    zel_level_add_component(example_level, entity, transform);

What is different however is the need to **register component types**.
That means if you want to use a Transform Component you need to register it.
Otherwise the engine does not know about the Component and it will **crash**.
You can do this at any time, but it is recommended to do this when creating a :ref:`Level<unity_scenes>`.

.. code-block:: cpp

    zel_level_register_component<zel_transform_t>(example_level);

For a more detailed **explanation about registering** components, please see the `Components section <https://nvriezen.github.io/ZelEngineDocs/basics/components.html#registering-components>`_ in the engine's documentation.

Getting Components
------------------
When you want to **access a variable** in a certain component, you first have to get a pointer to that component.
This is done in a somewhat similar way as with Unity's ``gameObject.GetComponent()``.
However you also need to pass the Level where the Entity exists in.

.. code-block:: cpp

    zel_transform_t* transform_component_ptr = zel_level_get_component<zel_transform_t>(example_level, entity);

There is also a way to only **check** if an entity has a certain **combination of components**.

.. code-block:: cpp

    zel_level_has_components<zel_transform_t, zel_material_t>(example_level, entity);

As you can see you can pass in only **one or multiple component types**.

Again, if you'd like to get a more detailed look at Components, look at the `engine's documentation <https://nvriezen.github.io/ZelEngineDocs/basics/components.html>`_.
