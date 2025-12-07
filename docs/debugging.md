# Bring Up / Debugging Guide

This page will cover the procedure for bringing the board online and debugging problems

## Tools for Debugging

Diagnostic Tools:
  * Mandatory
    * At minimum, a digital multimeter is necessary for basic tests.
  * Recommended
    * A Mega Everdrive Pro can run various diagnostic tests
      * These tests do not run properly on older Everdrives like the original Mega Everdrive
      * I am unsure if these tests run properly on the Mega SD
      * Available Tests:
        * [240p Test Suite](https://github.com/ArtemioUrbina/240pTestSuite)
        * [Krikzz mcdverificator](https://github.com/krikzz/mega-ed-pub/tree/master/mcd-verificator)
        * [megacdTest](https://github.com/fabiodl/megacdTest/tree/master)
  * Helpful:
    * It is extremely helpful to have access to an Oscilloscope
      * You should have a ground spring on your probe as an alligator clip will add too much noise to many signals.
    * Bench power can help you do basic tests while the board is isolated
      * You can inject 9V to SVCC and 5V to VCC to do basic tests on the clocks and VC rail.

Runtime Debugging:
  * A [debug length flex](https://ko-fi.com/s/c640f7d734) is extremely helpful.
    * This allows you to run the drive outside of the case for easily access to signals
    * If you do this, **please buy and wear laser safety googles** and do not ever look directly at the laser
    * Use a cell phone camera to take a video of the laser diode while looking away from it if necessary.
  * An [oscilloscope breakout board](https://ko-fi.com/s/566563e520) can help you attach the scope to the test points while the unit is upright.
    * Keep all wires short as possible and ground wire thick
    * Long wires will degrade signal
    * It is possible to attach this board where the locking clip on the case goes if you don't want to run the unit outside the case

## Bring Up Guide

Here we cover the checks you should do before first power on

### Before First Power On

Here we review the steps to check before you try powering on the system

In continuity mode on your multimeter: 
  * Check that there is no short between the following test points:
    * VCC <-> VC
    * VCC <-> GND
    * VC <-> GND
    * SVCC <-> SGND
  * Check that GND / SGND are shorted.
    * On versions of the board prior to -14 this requires a bodge wire.
    * This bodge is installed from the factory on stock boards between two of the test points

Limit Switch Checks:
  * Manually move the laser in position and home the laser by spinning the gears with your finger
    * S100 on the KSK should be short when the laser is homed, and open when its out and about
    * On the Eject Board...
      * The limit switch closest to the connector should be open when the drive is closed
      * The limit switch closest to the motor should be short when the drive is closed
    * On your first boot, ideally have the system in this state before power on.

## Debug Guide

Here we will walk through various scenarios and how to troubleshoot

### My tray is jammed and I can't eject

When installing the eject board, it is possible to jam the tray. This can source from a few different issues. 

To resolve this, let's perform a few checks:
  * Look at the limit switches on the eject board
    * On the switch closest to the connector (tray open), the leaf should be facing the back of the drive
    * On the switch closest to the motor (tray closed), the leaf should be facing the front of the drive
    * If these are inverted it can cause a jam.
  * The positioning of the closed leaf is important. There is a groove on the big gear that it aligns with. 
    * If you jostled the gear while inserting or removing the board, it can cause a jam. 
    * You may need to take the eject board or the gear off and realign it.
  * For OpenKSK specifically...
    * Make sure the board is sitting flush on the motor pins
      * If it is lopsided the height changes and may interfere with the mechanical aspects
  
### My tray is continuously ejecting

This is a very common problem with these drives. 

To resolve this, let's perform a few checks:
  * If you have not already replaced the belt, first remove the tray
    * Remove the two screws on the spindle magnet and take it off.
    * In the top right hand corner of the drive is a little plastic clip.
      * Take it off.
      * This is what hits the limit switch for tray open. Don't lose it.
    * Spin the large gear so that the laser lowers and the tray starts ejecting
      * When you reach max, there is two little pieces of plastic on either side.
      * Use a screw driver to gently open them while gently pulling
      * Tray will remove
    * Remove the black belt cover
      * Push in and pull up on the top tab of it
      * Slide to the left and then you can take it off 
      * There are two notch openings after the slide that can be seen from the bottom
    * Inspect the belt
      * The most common cause of this issue is belt failure
      * The belt needs to be very tight. If it's stetched this issue will occur.
      * To trigger the drive closed switch, a bit of torque is needed on the belt. 
      * Without it, the drive cannot raise to the final notch and will fail to hit the switch
      * If the switch has not been hit after several seconds longer than expected, the MCU ejects the drive thinking its stuck.
      * Replace the belt if necessary
  * Next inspect the eject board
    * When the drive is closed and the laser is in place, the limit switch closest to the motor should be shorted
    * If the value reads fairly low resistance but not shorted, the limit switch needs to be cleaned.
  * For OpenKSK specifically
    * Make sure the board is sitting flush on the motor pins
      * If it is lopsided the height changes and may interfere with the mechanical aspects
    * Check that the orientation of your CN2 cable is correct.
      * The red wire should be Pin 1
    * Check soldering on the sled lines (see schematic))    

### I hear a grinding noise when I turn the power on or close the drive tray

This is usually an issue with the S100 limit switch on the KSK board. The sound is the sled gear stripping when the motor does not stop upon reaching home from the limit switch signal.

To resolve this, let's perform a few checks:
  * Remove any disc from the drive
  * Spin the laser into home position while moving the gear with your finger
    * When homed, the legs of the limit switch should be shorted
  * Move the laser out
    * When out, the legs of the limit switch should be open
  * If the limit switch is not reading 0 when closed...
    * Ensure you've cleaned the switch. They get dirty over the years and make poor contact
    * You can carefully remove the cap to clean. I recommend an IPA bath.
    * Be careful not to bend teh leds. If you do you can damage the switch beyond repair
    * Restore the cap very gently, paying attention to the orientation
  * For OpenKSK specifically...
    * Ensure the orientation is correct. If it's backwards, it wont trip correctly
    * Ensure the switch is level. If it's lopsided, it wont trip correctly
    * Ensure the KSK is flush on the motor pins. If the height is too high, the switch won't trip.

### The disc is not spinning

The disc requires the following to start spinning:
  * Tray shut
  * Laser in position
  * "Open" Limit Switch open (closest to connector on eject board)
  * "Close" Limit Switch asserted (closest to motor on eject board)
  * Laser in home position
  * S100 limit switch shorted

You can verify the state of the limit switches by turning the power off and checking them with a multimeter.

If all of these are correct and the disc does not spin:
  * Check the soldering on the spindle lines on the schematic

### My KSK is not reading discs

First of all, determine if this is one or both types of disks. Games use a different logic path than Audio CDs, and it is possible for one to work but not the other.

If both are not working:
  * Put a disk in the drive and run krikzz's mcdverificator.  
    * WARNING: This will clear the backup RAM. Backup first.
    * If CD INIT Error 01, the laser cannot see the disc
      * This can be a focus issue or a dead laser
      * If you have the debug length flex...
        * Run the drive outside the case while wearing laser safety goggles
        * While not looking at the drive, flip on the power while recording it with a cell phone camera
        * Wait a few seconds and turn the power off. Watch the video
        * If you cannot see the laser diode turn on in the video, your laser may be dead and need replacement
        * For OpenKSK specifically, check soldering on LV/LDON
    * If CD INIT Error 03, the laser diode is alive and can see the disc
      * If you have an oscilloscope, attach to the RF point and verify the eye pattern
      * If you substituted any of the electrolytic capacitors on the BOM, this is an ESR sensitive circuit. 
        * Using random capacitors is a dice roll on whether you can read discs. 
        * Try using the recommended ones.
        * You may need to dial your laser with an oscilloscope.

If one type works but not the other:
  * Check soldering on PSSL
  * Verify audio clocks on the CXD: LRCK, BCLK (DA15), WFCK, and DA16 (DATA)
  * Verify soldering on SQCK/SQSO
  * Verify EXCK/SBSO

If you've adjusted the pots on the laser in the past:
  * Return them to normal position if you marked it
  * Pray you didn't burn out the laser diode with new parts + an amped beyond stock laser if you installed new ones

If all looks correct, try replacing laser.

### My discs can read, but I get disc read errors shortly after playing an Audio CD or starting a game

This usually happens for mechanical issues, not a problem with the drive board. Over time the mechanical rails and gears all try out.

If there is too much resistance on the rails, the sled will not be able to move with the fine grain stepping required to read all data. Regreasing the gears and rails can solve this problem. The sled should move smoothly with little resistance. Do not overgrease or the gunk buildup will itself become resistance.

You should also check if the gear is damaged. If you have stripped the gear due to other problems, then the missed step(s) will cause disc read errors. 3d models are available to fix many common problems

## I determined one or more of my mechanical parts are broken

The community has made many 3d models to fix problems:
  * [Helix Gear](https://www.printables.com/model/367421-sega-mega-cd-mk1-helix-drive-gear)
  * [Sled Gears](https://www.printables.com/model/638144-sony-optical-pickup-mechanism-kss-150-210-213-240-)
  * [Tray Limiter Tab](https://www.thingiverse.com/thing:5619264)
  * [Front Bezel](https://www.thingiverse.com/thing:7101856)
  * [LED Diffuser](https://www.printables.com/model/520094-sega-mega-cd1-led-diffuser)
  * [Spindle Magnet Holder](https://cults3d.com/en/3d-model/gadget/sega-cd-mega-cd-spindle-holder)

Models wanted:
  * Large gear that attaches to the helix gear.

# License

This documentation is offered under the [CC-BY-SA](https://creativecommons.org/licenses/by-sa/4.0/) license. Linked files may carry their own licenses.
