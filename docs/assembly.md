# Assembly

## Official Install Video

[1upRestorations](https://1uprestorations.com) has graciously provided an [official install video](https://youtu.be/wNfhOMQv8S0).

## Main Board

**Please carefully follow the following steps**

###Preparation

Perform these steps in preparation:

* Remove the KSK board from your Sega CD and disconnect the flex cable.
* Unplug the KSK from the eject board
* Remove the screw above R163
* Desolder the two motors. Then you shoudl be able to remove the KSK board from the mechanical assembly

### Remove Top Side Parts

Ideally, you should start by removing all the parts from the top (non-solder) side that would keep the board from sitting flush on a table

* Remove Limit Switch
* Remove Electylic capacitors
  * Note that this board is ESR sensitive
  * Do not replace this with random capacitors
  * Follow the bom or use a Console5 kit
  * Random capactiors may cause the board to malfunction
* Remove Connectors (CN1,CN2,CN3)
* Remove BA6294 BTL Driver
* Remove AN8377N Motor Driver

### SMD Passives

If you are transferring parts, I recommend moving all the passives at this point. If you have hot tweezers and pre-tin the pads, you can do this really quickly.

### Removing the Chips

Different batches seems to have different levels of epoxy on the chips. Some are really weak and some are superglued - it's impossible to know which you'll have.

The following is the safest strategy:
  * Ideally, flood all the pins of all the chips with ChipQuik low melt solder so that they are all shorted. Mix with the existing solder well. 
    * I recommend using a J-Tip so as to minimize the chance of bending legs.
  * If you have a hot plate or preheater, put the board on it and set the preheat to 175c
    * Put a bunch of flux on all the pins to lower the melting point further
    * After several minutes pass, start to poke at the chips
    * If they aren't coming up, push flux under the chip as it'll help break up the epoxy
    * Gently continue poking at them until the come off.
    * Slide the chips off and let them cool
  * If you only have a hot air station...
    * Be very careful. Keep the temperature lower than usual and continue circling without end
    * You need to be very careful not to burn the chips while breaking the epoxy
  * If you only have an uncontrolled hot air gun...
    * I don't recommend proceeding. If you burn the CXP5084 that is irreplacable and the unit is dead for good.
  * If you only have an iron...
    * Because of the epoxy you cannot remove these chips with just an iron.

Once all chips are safely removed, use goot wick to clean all of the legs

### Final Assembly

From here, I would proceed in the following order:
  * Solder QFPs
  * Solder Drivers
  * Solder Connectors
  * Solder Electrolyics
    * Do not substitue electrolytics with random capacitors
    * The circuit is ESR sensitive and many cap types will cause malfunctions
  * Solder Limit Switch

## Eject Board

**Please carefully follow the following steps**

### Removal

The following steps are required to remove the eject board:
  * Disconnect the wire harness
  * Desolder the motor
  * Pin the center clip and slowly start to lift up
  * Once that one is clear, pinch the one between the limit switchs to finish removing
    * Be careful not to damage the limit switches. Raise evenly.

### Transferring Parts

To move to the OpenKSK board...
  * Note the position fo the limit switchs
  * Desolder them
  * Clean them, as they probably need it
  * Remove the connector

Reinstall the parts on the new board

### Assembly

Reassemble by snapping the board into the two plastic clips, and then solder the motor.

## Before Power On

Please review the Bring-up / Debugging guide from the left navigation before powering on. This will help you avoid careless mistakes that in the worst case can damage parts.

### License

This documentation is offered under the [CC-BY-SA](https://creativecommons.org/licenses/by-sa/4.0/) license. Linked files may carry their own licenses.
