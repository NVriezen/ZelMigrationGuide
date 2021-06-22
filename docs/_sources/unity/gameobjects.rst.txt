.. _unity_gameobjects:

GameObjects
===========

In the **Unity** game engine the concept of **GameObjects** is used to represent things such as player characters or props in the world.
These **GameObjects** have **Components** attached to them which hold the data and logic.
**GameObjects** and **Components** always inherit from ``MonoBehaviour``.

Entities
--------
First I have to tell you the "equivalent" of the Game Object in the **Zel Game Engine**.

This engine does not use objects to represent player characters or props in the game world.
It rather uses **Entities** as a representation of those things.

In essence **Entities** are nothing more than a unique number. Components are then tied to this unique number, the entity. This can be used to create a sort of **Game Object** like you are used to. 

The important thing to note here is that **Components** in the **Zel Game Engine** do not have any logic, it's only data. See the :ref:`Components section<unity_components>` for more information on this.

.. admonition:: Why?
	:class: in-depth

	The reason to split the logic and data up is to make the code **easier to debug and optimize**.
	It can also make it **more modular**.
	This combined can increase a **programmers productivity and joy of programming**.

	It also makes it easier to pack the data closely together.
	This can help fit data inside the CPU cache and reduce wasting memory space.
	Which can also increase performance, since less data has to be loaded from RAM.

	Since all the logic and data is now placed inside Systems and Components respectively, the GameObjects are nothing more than a **container for Components**.
	However to represent a GameObject in this kind of way, we only need a **unique identifier**.
	This unique identifier or entity ID "owns" certain components and that is what makes it a player, enemy or just a tree.

	Let's say we want a player.
	We create an Entity, which will get the ID 16777217.
	Then we create some components like a player tag, transform and movement component.
	We **link** these components to that specific entity ID 16777217 by adding them to that entity.
	By looking at an entity's components we know what kind of entity it is, in this case a player.

	We could then easily change this player to an enemy.
	By removing the player tag component and attaching a component that represents enemies.
	This can greatly improve modularity, but also makes it easy to iterate over only enemies.
	Since we can look at all the entities with the specific enemy component.
	That's exactly what systems do.

	In essence, an entity is just **a unique number to which some data is associated**.
	However it gives us a lot of flexibility over the standard Object Oriented approach.

If you want to learn how to create Entities, please take a look at the `Entities section <https://nvriezen.github.io/ZelEngineDocs/basics/entities.html>`_ in the engine's documentation.

Transform
---------
**GameObjects** in **Unity** always have a name and a transform component.
In the **Zel Game Engine** you can still create this same setup yourself, since the source code is open. You can make changes however you like.

However in the **Zel Game Engine** when you create an **Entity** it has no components attached to it yet. You can easily attach the built in Transform Component to it. This can be done in the following way.

.. code-block:: cpp

	zel_transform_t transform{ { 0.0f, 0.0f, 0.0f },{ 0.0f, 0.0f, 0.0f },{ 1.0f, 1.0f, 1.0f } };
	zel_level_add_component(example_level, entity, transform);

Accessing Transform Component
-----------------------------
Normally in Unity when you have a **GameObject** you can do the following.

.. code-block:: cpp

	gameObject.transform.position.x = 0;

This is not how the **Zel Game Engine** works.
All data of **Entities** is stored in **Components**.
So you first need to get the component you need and then you can access the Transform data that way.

.. code-block:: cpp

	zel_transform_t* transform_component_ptr = zel_level_get_component<zel_transform_t>(example_level, entity);
	transform_component_ptr->position.x = 0;

For more detailed information about **Components** and accessing their data, please see the :ref:`Components section<unity_components>`.

Accessing GameObject
--------------------
In Unity you have several ways to find a particular GameObject.
For example, you can use ``FindObjectOfType()`` or do ``GameObject.Find()``.

The entities in the Zel Game Engine are just numbers, so we can not use names or anything.
We could store the entity ID and access an Entity in that way.
However if we don't have the entity ID, but we only want to run code for the player?
As further explained in the :ref:`Update section<unity_scenes>` systems can iterate over entities.
This is done by looking at the components an entity contains.

.. code-block:: cpp

	for (zel_entity_id entity : zel_entities_list<zel_transform_t>(level))
	{
			//use the transform component here
	}

When we give our player a tag we can easily find it through using the ``zel_entities_list``.

.. code-block:: cpp

	//This will go at the top of a file
	struct player_one_tag { };

	//Put this alongside you initialization code
	zel_entity_id player_entity;
	player_one_tag player1_tag;

	zel_level_register_component<player_one_tag>(example_level);
	zel_level_add_component(example_level, player_entity, player1_tag);

	//This code would be placed inside a system or basically be the system
	for (zel_entity_id entity : zel_entities_list<zel_transform_t, player_one_tag>(level))
	{
		//Now we only get player 1 as entity
		//So we can access player 1 and its transform component here.
		zel_transform_t* player1_transform = zel_level_get_component<zel_transform_t>(example_level, entity);
	}

It's also very easy when you somehow want another entity to become player 1.
Because you simply remove the tag from the current entity and add the player tag to the new entity.



