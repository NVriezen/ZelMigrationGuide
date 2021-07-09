.. _unity_update:

Update & Other Logic
====================
In Unity every class or Component that inherits from ``MonoBehaviour`` can use the ``Update`` function.
This is where you can put code which should run **every frame**.
Apart from ``Update`` there is also ``FixedUpdate`` and ``LateUpdate``.

The Zel Game Engine doesn't have update functions like ``FixedUpdate`` and ``LateUpdate``.
In fact, the engine approaches this concept a bit differently.

System
------
All logic is put inside **systems**.
When you need to move a character, you make a movement system.
Or when you want enemies to attack the player, you create an AI enemy behavior system.

As explained in other section of this guide, GameObjects in Unity contain data and logic.
In the Zel Game Engine we split this data and logic up.
Data is stored in components and the **logic runs from within systems**.
It keeps everything very modular and flexible.

.. admonition:: Why?
    :class: in-depth

    We store the logic inside systems to be able to run the **same logic on a lot of components**.
    In Object Oriented Programming you would look at a single object.
    How that object behaves and functions.

    When targeting a more Data Oriented approach you need to look at the bigger picture.
    Does this code only run on one entity?
    You probably realize that some code actually runs on multiple instances of that entity.
    For example there may be multiple enemies targeting the player.

    That is where these Systems come in.
    By putting that logic into a system you can manipulate those enemies **all at once**.
    Sometimes literally through **SIMD technology**.
    
    .. admonition:: SIMD
        :class: note

        Single instruction, multiple data (SIMD) is a class of parallel computers in Flynn's taxonomy. It describes computers with multiple processing elements that perform the same operation on multiple data points simultaneously (source: Wikipedia).

    You need to remember that instructions are also **data**.
    The instructions for the manipulation of the actual entity data is already in the **CPU's cache**.
    That means the CPU does not have to constantly load new instructions from RAM.
    That is a **very slow process** and therefore **reducing the amount of reading from RAM can increase performance**.


Differences
-----------
The difference between a System and Unity's ``Update`` function is actually not that significant.
Systems are also just functions.
However you need to **explicitly register** them. 
Being able to deregister them as well makes them a bit more powerful.

To register a system you can simply so the following.

.. code-block:: cpp

    void my_custom_system(zel_level_t* level, float delta_time)
    {
        zel_print("Print this every frame.\n");
    }

    zel_level_register_system(example_level, my_custom_system, "MyCustomSystem");

As you can see the system also has a **delta time** variable, just like in Unity.

Being able to deregister the system has the benefit of turning off certain logic.
Take for example an input system. Maybe the game should ignore player input for a couple seconds.
You can easily achieve this by deregistering the input system and after a couple seconds re-register it again.

.. code-block:: cpp

    zel_level_unregister_system(example_level, "MyCustomSystem");

.. tip::

	There is a template included to make it easier for you to design your own systems. Check out where you can find it, in the `documentation <https://nvriezen.github.io/ZelEngineDocs/basics/systems.html#template>`_

Manipulating Components
-----------------------
Running a program is in essence nothing more than manipulating data.
That's why the engine is somewhat Data Oriented and separates the logic from the data.
However when the data is stored in Components, how can a System **access this data**?
The simplest way to do this is with the entity collection of the engine and a for loop.

.. code-block:: cpp

	std::vector<zel_entity_id> entity_collection = zel_entity_collection_create<zel_transform_t>(level);
	for (size_t collection_index = 0; collection_index < entity_collection.size(); collection_index++)
	{
        	//use the transform component here
	}

You use the ``zel_entity_collection`` functions to specify which components an Entity must have.
Then the ``std::vector`` it returns will only contain the Entities which have those components added to them.
This means you can not only input one component type like above, but actually **multiple component types**.

.. code-block:: cpp

    	std::vector<zel_entity_id> entity_collection = zel_entity_collection_create<zel_transform_t, zel_material_t, zel_mesh_t>(level);
	for (size_t collection_index = 0; collection_index < entity_collection.size(); collection_index++)
	{
        	//Now you can use the transform and the material components here
	}

Accessing Specific Entities
---------------------------
You may want to only manipulate the transform components of all the enemies.
How can this be achieved?

You already know how to access specific components.
So to access certain components from specific entities you only need to tag them.
A tag can be just a simple empty struct.

.. code-block:: cpp

    struct enemy_t { };

Add this tag to all the enemies.
It's just like any other component.

.. code-block:: cpp

    //Don't forget to register the tag as a component
    zel_level_register_component<enemy_t>(example_level);
    
    enemy_t enemy_tag;
    zel_level_add_component(example_level, enemy1_entity, enemy_tag);
    zel_level_add_component(example_level, enemy2_entity, enemy_tag);

Then to access only the transform components of all the enemies you use the ``zel_entity_collection`` functions like normal.
This time asking for the ``zel_transform_t`` and ``enemy_t`` components.
Which will give you only enemy transform components, since only enemies have the enemy tag attached to them.

.. code-block:: cpp

	std::vector<zel_entity_id> entity_collection = zel_entity_collection_create<zel_transform_t, enemy_t>(level);
	for (size_t collection_index = 0; collection_index < entity_collection.size(); collection_index++)
	{
        	//Now you can manipulate all the transform components of the enemies.
	}

