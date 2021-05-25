.. _unity_update:

Update
======
In Unity every class or Component that inherits from MonoBehaviour can use the ``Update`` function.
This is where you can put code which should run every frame.
Apart from ``Update`` there is also ``FixedUpdate`` and ``LateUpdate``.

The Zel Game Engine doesn't have update functions like ``FixedUpdate`` and ``LateUpdate``.
In fact, the engine approaches this concept a bit differently.

System
------
All logic is put inside systems.
When you need to move a character, you make a movement system.
Or when you want enemies to attack the player, you create an AI enemy behavior system.

As explained in other section of this guide, GameObjects in Unity contain data and logic.
In the Zel Game Engine we split this data and logic up.
Data is stored in components and the logic runs from within systems.
It keeps everything very modular and flexible.

Differences
-----------
The difference between a System and Unity's ``Update`` function is actually not that big.
Systems are also just functions.
However you need to explicitly register them. 
Being able to deregister them as well makes them a bit more powerful.

To register a system you can simply so the following.

.. code-block:: cpp

    void my_custom_system(zel_level_t* level, float delta_time)
    {
        zel_print("Print this every frame.\n");
    }

    zel_level_register_system(example_level, my_custom_system, "MyCustomSystem");

As you can see the system also has a delta time variable, just like in Unity.

Being able to deregister the system has the benefit of turning off certain logic.
Take for example an input system. Maybe the game should ignore player input for a couple seconds.
You can easily achieve this by deregistering the input system and after a couple seconds re-register it again.

.. code-block:: cpp

    zel_level_unregister_system(example_level, "MyCustomSystem");

Manipulating Components
-----------------------
Running a program is in essence nothing more than manipulating data.
That's why the engine is somewhat Data Oriented and separates the logic from the data.
However when the data is stored in Components, how can a System access this data?
The simplest way to do this is with a special iterator and for loop.

.. code-block:: cpp

    for (zel_entity_id entity : zel_entities_list<zel_transform_t>(level))
    {
            //use the transform component here
    }

You use the ``zel_entities_list`` to specify which components an Entity must have.
Then the list will contain an iterator which only return the Entities which have those components added to them.
This means you can not only input one component type like above, but actually multiple component types.

.. code-block:: cpp

    for (zel_entity_id entity : zel_entities_list<zel_transform_t, zel_material_t, zel_mesh_t>(level))
    {
            //Now you can use the transform and the material components here
    }

Why?
----
The reason why the engine uses Systems is to reduce the amount of cache misses.
Instructions are also data and they are put into instruction cache before they are ran by the CPU.
Reducing the amount of times to fetch instructions from main memory(RAM) can greatly reduce the time it takes to run your code.
