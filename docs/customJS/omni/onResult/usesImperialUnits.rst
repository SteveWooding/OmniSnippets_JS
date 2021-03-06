.. _usesimperial:

Check if imperial units are default
-----------------------------------

A function that returns a :ref:`boolean<bool>` informing about the default measurement system in the user's country.

.. note::

    If you want to obtain the location of the user, the recommended function is :ref:`getCountryCode<getCC>` available inside :ref:`onInit<onInit>` context.

The function doesn't inform about the current choice of units but only about
the default system of measurement (imperial vs metric).

Syntax
~~~~~~

To check whether the user's country follows the imperial system of units or not
and save the resulting boolean in a variable use:

.. code-block:: javascript

    var defaultImperial = ctx.usesImperialUnits();

.. warning::

    This function only works inside a ``onResult`` context.

