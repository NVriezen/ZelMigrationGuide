.. _getting_started_installation:

Installation
============
This page describes the process to install the Zel Game Engine on your machine.

Downloading The Engine
----------------------
First you need to download the required files.
This can be done by cloning the Github repository or just downloading it directly.

If you've never used Github before the recommended option is to go to :ref:`downloading_rep`

Cloning The Repository
^^^^^^^^^^^^^^^^^^^^^^
Cloning a repository won't be explained in this documentation.
Luckily there are plenty of resources out there including `Github's own documentation`_.

.. _Github's own documentation: https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/cloning-a-repository

The engine's repository can be found `here <https://github.com/NVriezen/ZelGameEngine>`_.
It has most of the files needed for the engine.

When you have cloned the repository, you can skip to :ref:`setting_up_glfw`.

.. _downloading_rep:

Downloading The Repository
^^^^^^^^^^^^^^^^^^^^^^^^^^
Downloading the engine is rather simple.
Go to `the engine's repository <https://github.com/NVriezen/ZelGameEngine>`_.
Then click on the green **code** button and click on **Download Zip** in the dropdown menu.
This way you'll download the latest master version of the engine.

.. _setting_up_glfw:

Downloading GLFW
----------------
The engine uses GLFW to draw a window and manage inputs for us.
We need to download GLFW and set it up in Visual Studio correctly.

Download the correct pre-compiled binaries for your machine `in GLFW's download section <https://www.glfw.org/download.html>`_.

.. note:: You can build the binaries yourself if you want. Make sure you build GLFW 3.3.2 or higher.

Extract the folder in the zip file somewhere on your machine.
Preferably in a location where you can use it for other projects as well.

Now we have all files we need to be able to build the engine.
The only thing left is setting it up correctly in Visual Studio.

.. _setting_up_vs:

Setting Up Visual Studio
------------------------
Right now the engine has only been used with Visual Studio 2015.
Other build environments won't be supported anytime soon.
Any Visual Studio version from VS2015 onwards should be good though.

Open the solution through ``ZelGameEngine.sln``
After Visual Studio loading the solution, you'll see two projects.
One named ``Application`` and another named ``ZelGameEngine``.

Directories Setup
^^^^^^^^^^^^^^^^^
Right-click on the ``ZelGameEngine`` project and select ``Properties``.
This should open a window where you can change the configuration properties.
In the top left set the configuration to ``All Configurations``.
After that select ``VC++ Directories`` in the left column.
Here we need to set up GLFW for the project.

Include
"""""""
Click the dropdown button in the ``Include Directories`` row on the right and then "Edit...".
You'll see something like ``$(SolutionDir)..\..\GLFW\glfw-3.3.2.bin.WIN64\include``.
Change this path to where you stored the include folder of GLFW for the selected platform (in this example Windows 10 64-bit).

Library
"""""""
Do the same for the ``Library Directories`` row as you did for the ``Include Directories`` one.
Change the path to point towards the correct ``lib-`` folder based on your Visual Studio version.

Great! GLFW should be setup now.

Configurations
^^^^^^^^^^^^^^
The engine works with two configurations: ``Debug`` and ``Release``.
Some features may be disabled in the Release configuration.
The output console, where all messages from ``zel_print`` will appear, is one of them.

Testing the setup
^^^^^^^^^^^^^^^^^
At this point you should be able to run the project.
Be sure to set the configuration on ``Debug``.

If everything went well, you won't be seeing any build errors.
A somewhat dark (blue) window should appear with a command prompt window.
In the command prompt window you can see the engine's version as well as "Hello World".

.. note:: 
    If you do get errors or any other issue, please **check again** if you followed the instructions correctly. If the problems persist, please open an issue on the documentation's `Github issues page <https://github.com/NVriezen/ZelEngineDocs/issues>`_ describing the problem you have.

What's Next?
------------
Now that you have everything setup, you can create your game.
You may check out the :ref:`Examples<examples>` section to help you get underway.
Otherwise check out the :ref:`API Reference<api>`.