Stepping it up a notch
======================

Now that you have a basic program, why not work on adding a few more advanced things.

Single-joystick control
-----------------------

Sometimes it is preferable to control the bot using a single joystick.

Earlier we had:

.. code-block:: c

    motor[leftMotor]  = vexRT[Ch3];   // Left Joystick Y value
    motor[rightMotor] = vexRT[Ch2];   // Right Joystick Y value
    
This allowed us to use the left and right joysticks to control their corresponding wheels.
However, let's say we only wanted to use the left joystick, more like a video game controller is used to move the player.

Here, we will use some math the calculate the speed at which each wheel should move:

.. code-block:: c

    motor[leftMotor]  = vexRT[Ch3] + vexRT[Ch4];  // Left Joystick Y value + Left Joystick X Value
    motor[rightMotor] = vexRT[Ch3] - vexRT[Ch4];  // Left Joystick Y value - Left Joystick X Value
        
As you can see, we are adding :code:`Ch4` (x-axis) to :code:`Ch3` (y-axis) to move the left motor, 
and we are subtracting :code:`Ch4` from :code:`Ch3` to move the right motor.

This works because as we push the joystick further right (a positive x-value) we increase the power of the
left motor while decreasing the power of the right motor, turning the bot rightwards, and vice versa.

Thresholds
----------

The joysticks are not perfect, and sometimes they will report value (albiet small) even when untouched.
To prevent this, we can implement a threshold to make sure the wheels (or anything else that uses joystick values)
only move when the joystick is pushed past a certain value.

First we will create a function called :code:`threshold` at the bottom of our code (below :code:`task main`):

.. code-block:: c

    int threshold(int value, int threshold)
    {
	    return abs(value) > abs(threshold) ? value : 0;
    }
    
You may be wondering what the :code:`? :` is, that is a ternary operator, it works essentially like an inline if-else statement.
They can help us to write out code in a shorter amount of lines, however they should only really be used 
for basic things. You should never but a ternary operator in a ternary operator.

Next we need to declare our function at the top of our code, since it is at the bottom, 
any code in :code:`task main` that references it will throw an error since to compiler does not know
the function exists until it reaches the bottom of the code. A declaration is our way of telling the compiler
"don't worry, I'll define it later".

So, at the top of your code (right above :code:`task main`), simply add:

.. code-block:: c

    int threshold(int value, int threshold);
    
Now we can use our new function in our code! It takes two inputs, the value (an integer), 
the threshold (an integer) and it outputs an integer (the value if it is greater than the threshold
or 0 if not).

An example of how we would use it is:

.. code-block: c

    motor[leftMotor]  = threshold(vexRT[Ch3], 15);
    motor[rightMotor] = threshold(vexRT[Ch2], 15);    
