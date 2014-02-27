
kOS Mod Overview
================

kOS is a scriptable autopilot Mod for Kerbal Space Program. It allows you write small programs that automate specific tasks.

Installation
------------

Like other mods, simply merge the contents of the zip file into your Kerbal Space Program folder.

Usage
-----

Add the Compotronix SCS part to your vessel; it’s under the “Control” category in the Vehicle Assembly Building or Space Plane Hanger. After hitting launch, you can right-click on the part and select the “Open Terminal” option. This will give you access to the KerboScript interface where you can begin issuing commands and writing programs.

KerboScript
===========

KerboScript is a programming language that is derived from the language of planet Kerbin, which _sounds_ like gibberish to non-native speakers but for some reason is _written_ exactly like English. As a result, KerboScript is very English-like in its syntax. For example, it uses periods as statement terminators.

The language is designed to be easily accessible to novice programmers, therefore it is case-insensitive, and types are cast automatically whenever possible.

A typical command in KerboScript might look like this:

    PRINT “Hello World”.

Expressions
-----------

KerboScript uses an expression evaluation system that allows you to perform math operations on variables. Some variables are defined by you. Others are defined by the system.

There are three basic types:

### Numbers

You can use mathematical operations on numbers, like this:

    SET X TO 4 + 2.5.
    PRINT X.             // Outputs 6.5

The system follows the order of operations, but currently the implementation is imperfect. For example, multiplication will always be performed before division, regardless of the order they come in. This will be fixed in a future release.

### Strings

Strings are pieces of text that are generally meant to be printed to the screen. For example:

    PRINT “Hello World!”.

To concatenate strings, you can use the + operator. This works with mixtures of numbers and strings as well.

    PRINT “4 plus 3 is: “ + (4+3).

### Directions

Directions exist primarily to enable automated steering. You can initialize a direction using a vector or a rotation.

    SET Direction TO V(0,1,0).         // Set a direction by vector
    SET Direction TO R(0,90,0).        // Set by a rotation in degrees

You can use math operations on Directions as well. The next example uses a rotation of “UP” which is a system variable describing a vector directly away from the celestial body you are under the influence of.

    SET Direction TO UP + R(0,-45,0).  // Set direction 45 degress west of “UP”.

Command Reference
=================

* [Math](/KOS_DOC/command/math)
    * Basic Functions
    * Trigonometric Functions

* [Flight](/KOS_DOC/command/flight)
    * ADD
    * REMOVE
    * STAGE
    * WARP

* [File IO](/KOS_DOC/command/file)
    * COPY
    * DELETE
    * EDIT
    * LOG.. TO
    * RENAME
    * REMOVE
    * RUN
    * SWITCH.. TO

* [Flow Control](/KOS_DOC/command/flowControl)
    * BREAK
    * IF
    * LOCK
    * ON
    * UNLOCK
    * UNTIL
    * WAIT
    * WHEN.. THEN

* [Terminal](/KOS_DOC/command/terminal)
    * CLEARSCREEN
    * PRINT
    * PRINT.. AT
    * REBOOT
    * SHUTDOWN

Structure Reference
===================

Structures are variables that can contain more than one piece of information.  Structures can be used with SET.. TO just like any other variable.
Their subelements can be accessed by using : along with the name of the subelement.

* [Atmosphere](/KOS_DOC/structure/atmosphere)
* [Body](/KOS_DOC/structure/body)
* [Direction](/KOS_DOC/structure/direction)
* [Engine](/KOS_DOC/structure/engine)
* [GeoCordinates](/KOS_DOC/structure/geocordinates)
* [List](/KOS_DOC/structure/list)
* [Node](/KOS_DOC/structure/node)
* [Orbit](/KOS_DOC/structure/orbit)
* [Time](/KOS_DOC/structure/time)
* [Vector](/KOS_DOC/structure/vector)
* [Vessel](/KOS_DOC/structure/vessel)

Flight Statistics
=================

You can get several useful vessel stats for your ships

    ALTITUDE
    ALT:RADAR           // Your radar altitude
    BODY                // The current celestial body whose influence you are under
    MISSIONTIME         // The current mission time
    VELOCITY            // The current orbital velocity
    VERTICALSPEED
    SURFACESPEED
    STATUS              // Current situation: LANDED, SPLASHED, PRELAUNCH, FLYING, SUB_ORBITAL, ORBITING, ESCAPING, or DOCKED
    INLIGHT             // Returns true if not blocked by celestial body, always false without solar panel.
    INCOMMRANGE         // returns true if in range
    COMMRANGE           // returns commrange
    MASS
    MAXTHRUST           // Combined thrust of active engines at full throttle (kN)
    VESSELNAME

### Vectors

These return a vector object, which can be used in conjuction with the LOCK command to set your vessel's steering.

    PROGRADE
    RETROGRADE
    UP				// Directly away from current body

### Orbit geometry values

These values can be polled either for their altitude, or the vessel's ETA in reaching them. By default, altitude is returned.

    APOAPSIS			// Altitude of apoapsis
    ALT:APOAPSIS		// Altitude of apoapsis
    PERIAPSIS			// Altitude of periapsis
    ALT:PERIAPSIS		// Altitude of periapsis
    ETA:APOAPSIS		// ETA to apoapsis
    ETA:PERIAPSIS		// ETA to periapsis

### Maneuver nodes

    NODE                // Direction of next maneuver node, can be used with LOCK STEERING
    ETA:NODE            // ETA to active maneuver node
    ENCOUNTER           // Returns celestial body of encounter
    NEXTNODE            // Next node in flight plan.

## Resources

### Resource Types

    LIQUIDFUEL
    OXIDIZER
    ELECTRICCHARGE
    MONOPROPELLANT
    INTAKEAIR
    SOLIDFUEL

### Stage specific values

    STAGE:LIQUIDFUEL            // Prints the available fuel for all active engines.
    STAGE:OXIDIZER

### Global values

    PRINT <LiquidFuel>.                         // Print the total liquid fuel in all tanks. DEPRECATED
    PRINT SHIP:LIQUIDFUEL.                      // Print the total liquid fuel in all tanks.
    PRINT VESSEL("kerbRoller2"):LIQUIDFUEL.     // Print the total liquid fuel on kerbRoller2.
    PRINT TARGET:LIQUIDFUEL.                    // Print the total liquid fuel on target.


Flight Control
==============

These values can be SET, TOGGLED, or LOCKED. Some values such as THROTTLE and STEERING explicity require the use of lock.

### Controls which use ON and OFF

    SAS				// For these five, use ON and OFF, example: SAS ON. RCS OFF.
    GEAR
    RCS
    LIGHTS
    BRAKES
    LEGS
    CHUTES	// Cannot be un-deployed.
    PANELS

### Controls that can be used with TOGGLE

    ABORT
    AGX             // Where x = 1 through 10. Use toggle, example: TOGGLE AG1.

### Controls that must be used with LOCK

    THROTTLE			// Lock to a decimal value between 0 and 1.
    STEERING			// Lock to a direction.
    WHEELTHROTTLE       // Seperate throttle for wheels
    WHEELSTEERING       // Seperate steering system for wheels

System Variables
==========================
Returns values about kOS and hardware

    PRINT VERSION.            // Returns operating system version number. 0.8.6
    PRINT VERSION:MAJOR.      // Returns major version number. e.g. 0
    PRINT VERSION:MINOR.      // Returns minor version number. e.g. 8
    PRINT SESSIONTIME.        // Returns amount of time, in seconds, from vessel load.
