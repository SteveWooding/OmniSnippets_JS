Getting values of variables
===========================

When writing our customJS code we have several options to get the value of a calculator variables into the javascript code. Depending where you want to get the value, one or all options will be available to you. Let us talk in this section about the 2 most used ones (``toNumber()`` and ``ctx.getNumberValue``), how they differ, and when it is best to use them.

Get number value using ``ctx``
------------------------------

Probably the most used way to get the value of a calculator variable is using ``ctx.getNumberValue``. You can learn more about using it in the :ref:`Get numerical value of a variable<getnumval>`. This function is only available inside of ``onResult`` contexts. 

Its usage can cause problems since a variable without a value will return ``undefined`` which is a special javascript type that does not correspond to a numerical value.

.. note::
  When using ``ctx.getNumberValue`` your variable can have value ``undefined`` if the variable doesn't yet have a value in the calculator.

We can prevent erroneous behaviour by checking for the value of potentially ``undefined`` variables using ``if`` statements. Sometimes, however it is possible to use javascript implicit conditions to set a default value for our variables. Here is an example in which, if the variable ``'age'`` in our calculator has no associated value yet, the corresponding javascript variable ``age`` will not be ``undefined`` but ``0`` (zero).

.. code-block:: javascript

  var age = ctx.getNumberValue('age') || 0;

We will talk another day about :ref:`implicit conditions in javascript<implicitConditions>`. For now you just need to know that this line of code is the same as writing: 

.. code-block:: javascript

  var age;
  if (ctx.getNumberValue('age') {
    age = ctx.getNumberValue('age');
  } else {
    age = 0
  }

The only thing to keep in mind is what default value you can use (if any) without causing further problems in your code.

.. tip:: 
  If you really want to call ``ctx.getNumberValue`` from another function you can do so, but you need to call uch function from inside ``onResult`` and make sure to pass ``ctx`` as an input.

If you want to make sure you never get an ``undefined`` value in your variables (and your code is never executed if a variable doesn't have a numerical value yet) we have the ``toNumber()`` method.


Using ``toNumber()``: when and why
----------------------------------

In contrast to ``ctx.getNumberValue`` the ``toNumber()`` method requires that the variable of interest is passed as an input to your function. It means that you need to "plan ahead". It also means you can use it inside ``omni.define`` functions. 

.. note:: 
  When passing calcualtor variables as inputs, the functions will only be called and executed if all the inputs have a defined value. This is true for ``omni.onResult`` as well as ``omni.define``.

When using it in your ``omni.define`` function, you simply pass the calculator variable (use the name you gave it in the equations tab) as an input to your omni function. Before using this variable as a number you must call ``toNumber()`` on them. Here is an example:

.. code-block:: javascript

  omni.define('totalMuniez', function (money_earned, money_won) {
    money_earned = money_earned.toNumber();
    money_won = money_won.toNumber();

    return money_earned + money_won;
  });

The reason we need to use ``toNumber()`` is that variables inside the calculator are `decimal.js <https://github.com/MikeMcl/decimal.js/>`__ objects. These are designed to mimic decimal number better than the standard javascript ``Number`` type. If you wanted to operate with them you'd need to use their own internal methods. In our calculators, we almost never need anything more than the javascript ``Number`` type, so we make it a rule to play it safe and always use ``toNumber()`` to avoid confusion and problems.

When inside an ``onResult`` context we can pass variables as inputs but only if we have set them as triggers to the context. This ensure that our ``toNumber()`` call will never return a non-numerical value.

.. warning::
  Make sure you include all inputs to ``onResult`` as triggers (except ``ctx``) and that you do so in the same order in both places. The order in which the triggers are set defines the order of the variables and changing it in the inputs will create a miss-match betwen variable names and values (a.k.a. a huge mess).
