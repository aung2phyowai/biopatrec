# BioPatRec and Movements #

BioPatRec has currently the following output options:

  * [VRE](VRE.md)
  * DC motors
  * Servo motors
  * Standard prosthetic units

In order to know which movements are being used, an object is created during initialization. This object is then used to retrieve the values for what is to be sent to different outputs ([VRE](VRE.md) or Motors) in order to perform that specific movement.

New movements and their respective hardware configuration can be added by modifying the movements.def and motor.def files.

NOTES:
  * If you are different prosthetic hardware, don't forget to modify the movements.def and motor.def files if necessary.
  * We are now using an object-oriented way to process the output movements. However, it is not absolutely necessary to use these objects and these can be bypassed. See notes at the bottom of this page.

# Protocol #

The protocol which is used concurs with that of the [VRE\_Protocol](VRE_Protocol.md) and [Motors\_Protocol](Motors_Protocol.md). Please see each page for their specific protocol information.

## Movement Object ##

As stated above, each movement has its own object with the needed values for each outputting device.

The movements are initialised in the _Load\_patRec_ function by _InitMotors_. They are retrieved from _movements.def_ where each line represents a movement.

For each line the movements follow this standard:

|#1|#2|#3|#4|#5|
|:-|:-|:-|:-|:-|
|id|name|vre byte|direction|motor object index|

Each movement is then created using the movement class (movement.m).

The last parameter is an identifier to state which motor is connected to the movement, this is then activated when movement is performed. It can be represented as a space-delimitated list for multiple motors. The motor is identified by the index that it has in the definition file.

## Motors Object ##

The motors are initialised in the _Load\_patRec_ function, before the movements. They are retrieved from _motors.def_ where each line represents a motor and its position or speed during activation (in percentage).

|#1|#2|#3|
|:-|:-|:-|
|id|type|percentage of position or speed|

Each movement is then created using the motor class (motor.m).

**NOTE: If the order is changed, please change the _movements.def_ as well as this will point to the wrong motor otherwise.**

# Using the Objects in GUI #
In the GUI for testing Pattern Recognition Mov2Mov, when a button is pressed, the value of the dropdown-list is used to identify which movement in the list of movements is to be performed. Then depending on which types of movements are desired, the values in the objects are retrieved and sent to the correct output.

When the movements and/or motors are changed, such as when position is updated, it is important to overwrite the instance of that movement and/or motor in the handles. This due to MATLAB not transferring the same instance of the object in functions, but rather a copy of it.

# Functions roadmap #

  * Load\_patRec
    * InitMotors
      * motor
    * InitMovements
      * movements

# Notes #

  * In order to use the object-oriented way the last parameter of the _Load\_patRec_ function must be set to 1. This will load the names of the created movements into any dropdown menu with a given id of 'pm\_m?', where ? is a value from 1 to 20, in the new GUI being loaded.

  * There is another GUI for testing the real-time patrec which is not using neither the movements or motors objects (GUI\_TestPatRec\_Mov2MotDir). This relays in hard-coded relationships between the movements index and the assigned motor direction in the drop lists. Since we haven't used this GUI for a while, the real-time routines might need to be updated to the ones used in the GUI\_TestPatRec\_Mov2Mov.