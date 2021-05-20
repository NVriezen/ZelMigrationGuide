.. _input:

Input
=====
Currently the engine only has support for keyboard input.
You can not use the mouse in any way yet as well as joysticks.

Available Keys
^^^^^^^^^^^^^^
The keyboard keys that can be used are defined in the ``zel_PLATFORM_input_constants.h`` file.
Where ``PLATFORM`` is the platform you are building for.
Currently this can only be ``windows``.
These files can be found in ``ZelGameEngine/platforms/PLATFORM/include``.

Different Key Presses
^^^^^^^^^^^^^^^^^^^^^
When pressing and releasing a key, there are three phases.
The first frame the key is pressed can be detected with ``zel_input_get_key_press``.
Then all the following frames the key is still pressed can be detected with ``zel_input_get_key_hold``.
When releasing the key, the first frame it is released can be detected with ``zel_input_get_key_release``.

Key Press
---------
Key press only happens when during the previous frame the key was not pressed and the current frame it is.

.. code-block::

	int state = zel_input_get_key_press(ZEL_KEY_A);
	if (state)
	{
		zel_print("Key A is pressed");
	}

Key Hold
--------
When the key was held during the previous and current frame the key is marked as being held.

.. code-block::

	int state = zel_input_get_key_hold(ZEL_KEY_A);
	if (state)
	{
		zel_print("Key A is being held down");
	}

Key Release
-----------
If a key is released and the previous frame it was still pressed then Key Release occurs.

.. code-block::

	int state = zel_input_get_key_release(ZEL_KEY_A);
	if (state)
	{
		zel_print("Key A is released");
	}
