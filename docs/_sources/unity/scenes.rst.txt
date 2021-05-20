.. _unity_scenes:

Scenes
======
In Unity you would make your game inside one or multiple Scenes.
These scenes can represent a single town, a single stage or something like the main menu of a game.
Different scenes can be loaded at the same time or one after another.

In the Zel Game Engine this concept of scenes is represented in **Levels**.
A Level contains Entities, Components and Systems.
They all need to be created or registered in order to be used inside a Level.

Multiple Scenes
---------------
It is possible to create **multiple Levels** just like in Unity.
The important thing to note here is that Entities or Components can **only access data from within their own Level**.
So if the player want to access an Enemy from a different Level than the player is in, that is not possible.

Also there can only be **one active level** at the moment through the ``active_level`` variable.
You can write your own level manager to be able to use multiple Levels at once.

Scene Manager
-------------
For the moment there is no Level Manager in the Zel Game Engine.
Loading and unloading levels is done through ``zel_level_create`` and ``zel_level_destroy``.

There must always be a level assigned to ``active_level``.
So when switching levels, this needs to be done in **one frame**.
Destroying the old level and immediately creating the new one.

Running a Level
---------------
When running a Scene in Unity all the ``Update``, ``FixedUpdate`` and ``LateUpdate`` functions get called together with all kinds of other functions.
In the Zel Game Engine only ``zel_logic`` and ``zel_render`` get called.
As the names may suggest ``zel_render`` is for all your rendering code.
While all Systems that you have registered in a Level are called in ``zel_logic``.

You can decide what happens inside these functions.
So if you decide you want to do it a different way, that is totally possible.
Have a look at the :ref:`Update section<unity_update>` for more information.

