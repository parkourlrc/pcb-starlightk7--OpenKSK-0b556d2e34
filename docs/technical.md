# KSK Technical Overview

This document will attempt to explain the fundamentals of how this board works to the best of my knowledge. 

Please Note: I am hobbyist, not a professional engineer, so there may be mistakes in my understanding. However, I am offering this knowledge to the community as the result of my research in making this board. I hope it will help you understand not only how to build one, but how to troubleshoot and repair problems with stock units.

Feel free to ask questions or report anything you believe is inaccurate. 

In this document, we'll cover what each chip on the board does and how they communicate with each other. More details will be added over time as my research progresses.

## Sega CD Model 1: KSK-1200A (1-640-117-14)

* Power Rails
  * SVCC: +9V
    * Used for driving the spindle motor, tray motor, and tracking diode on the laser by the AN8377N
    * Used to drive the sled and the focus diode on the BA6294 BTL driver.
    * When motors are in operation, SVCC can droop to about 7.5V under load.
  * VCC: +5V
    * Powers the main logic chips (CXP-5084 MCU, CXA1372AQ Servo, CXD2500A DSP).
    * Powers the L358 op-amp in single supply mode.
  * VC: +2.5V
    * Center rail for the analog circuitry.
    * It is extremely important that this rail is functioning correctly
      * The board will function erratically if this line is unstable or incorrect voltage.
    * Derived from the +5V line
      * VC = (VCC + GND) / 2

* Clocks
  * Master clock is 16.9344mhz Crystal at X100
    * Mounted to CXD2500A Pins 53 (XTAI) and 54 (XTAO)
    * Routed to the mainboard via Pin 58 (C16M)
      * EMI Filtered at F100
      * AC Coupled at C134
      * Biased to VC Line at R165
      * Connected at CN3 Pin 1
    * Note: If you replace this Crystal with another part, you must retune the load capacitors
      * Check the datasheet of the crystal for the rated load capacitance
      * C108/C109 are the load capacitors
        * CL = (C108 * C109) / (C108 + C109) * CStray
        * C108 = C109 = 2(CL - CStray)
        * CStray is often approximated at 5pf
        * Use 1-2% tolerance COG/NP0 capacitors
  * MCU System Clock is Derived by CXD2500A
    * 4mhz clock is output on Pin 57 of the CXD2500AQ
    * Routed through R154 to Pin 61 (EXTAL) of the CXP5084
    * Test Point CLK1 exposes this signal near the empty X101
  * Serial Data Clocks
    * CXP5084 Pin 5 (PX0) -> CXD2500AQ Pin 74 (CLOK)
      * Used by CXD2500AQ for reading DATA (Pin 71)
    * CXP5084 Pin 5 (PX0) -> CXD2500AQ Pin 67 (SQCK)
      * Used by CXD2500AQ for reading SQSO (Pin 66)
    * CXD2500AQ Pin 79 CLKO -> CXA1372AQ Pin 22 (CLK) 
      * Used by CXA1372AQ to read it's DATA (Pin 24)
    * CN3 Pin 9 (EXCK) -> CXD2500AQ Pin 65 (EXCK)
      * Used by CXD2500AQ to read SBSO (Pin 64)
    * CXD2500AQ Pin 35 (DA15) -> CN3 Pin 15 (BCLK)
      * Used by the mainboard's LC8951 to read DATA (CXD2500AQ pin 34, CN3 Pin 14)
    * CXD2500AQ Pin 32 (LRCK) -> CN3 Pin 13 (LRCK)
      * Used by the mainbord's LC8951 to read audio channel data
    * CXD2500AQ Pin 62 (WFCK) -> CN3 Pin 4 (WFCK)
      * Used by the mainbord's LC8951 to read audio frame data

* Test Points
  * Power:
    * VC: +2.5V
    * VCC/5V: +5V
    * SVCC: +9V
    * GND: Ground
  * CXD2500AQ
    * M0: MNT0 - Error Correction Code Bit 0. Read out with Logic Analyzer.
    * M1: MNT1 - Error Correction Code Bit 1. Read out with Logic Analyzer.
    * M2: MNT2 - Error Correction Code Bit 2. Read out with Logic Analyzer.
    * M3: MNT3 - Error Correction Code Bit 3. Read out with Logic Analyzer.
    * SUBQ: Serial data for MCU/DSP communication
    * SQCK: Clock for reading SQSO Serial Data
    * SENS: SENS Data sent to CXP5084
    * CLKQ: DSP/Servo Serial Clock
    * FOK: Focus Ok
    * MDP: Spindle Motor Base Signal
  * CXP5084
    * CLK: 4mhz clock derived from DSP
    * GFS: Set to high when Frame sync / timing of intepolation protection lines up
    * LDON: Laser Diode ON signal
    * L0: Tracking Error Data, Format Unknown
    * L1: Tracking Error Data, Format Unknown
    * L2: Tracking Error Data, Format Unknown
    * PC2: 5V Power (PC2 pin is hard tied to 5V on this model)
    * PD3: Functon unknown, but this bus is Laser/Reset/CXD Latch
    * PE2: MCU/Mainboard serial data
    * PY1: Probably mute related since that's served on PY0'
    * RST: Reset Signal
    * SCOR: High during subcode sync S0 or S1
    * TE2: Tracking Error
  * CXA1372AQ
    * RF: Eye pattern signal
    * DFECT: Defect signal

* MCU: CXP5084-642Q
  * The firmware is burned into mask rom at the time of manufacturing. 
    * The package code 642Q represents the firmware version.
    * It cannot be swapped with a CXP containing other firmware versions.
    * There is currently no known way to dump the firmware.
  * Runs with a System Clock of 4mhz
    * Supports having a dedicated crystal at X101, which is unpopulated
    * Derives clock from DSP Pin 57 instead, which is connected through R157
  * Controls Reset On PD0
    * Runs through the SOT diode at D100 
      * Joins to its own RST pin at Pin 3
      * Connects to the Motherboard Reset Line at CN3 Pin 22
    * Signals Reset to the CXA1372AQ Servo via is XRST Line at Pin 25
    * Signals Reset to the CXD2500AQ DSP via is XRST Line at Pin 70
  * Communicates with the main board through the chip's 4-bit PA bus.
    * Pins 21-24 on the CXP are PA0-3
    * These connect to the mainboard on Pins 17-20 of CN3, which is DB0-3
    * If the CXP is malfunctioning or the bus is shorted, the Sega CD BIOS will hang without fully booting.
  * Controls the tray eject function and tray motor in conjunction with the AN8377N
    * Pin 17, PB0, signals the AN8377N to drive the tray via its channel on Pin 7.
    * Pin 19, PB2, connects to the outer limit switch on the eject board (closest to connector)
      * This switch tells the CXP5084 that the drive tray is fully open
    * Pin 20, PB3, connects to the inner limit sitch on the eject board (closet to the motor)
      * This switch tells the CXP5084 that the drive tray is fully closed and the laser is in position
      * If the belt is bad, this switch will fail to trip because there will not be enough torque to pull the laser in position
      * If this switch fail to assert, the try will automatically eject after a few seconds, causing the eject loop commonly seen with bad belts.
  * Controls the Laser Diode in Conjunction with the L358
    * Receives LV (Laser Voltage) signal from the L358 when the VC power rail has stabilized.
    * Emits LDON Signal to turn on the Laser Diode on Pin 1 (PD2)
  * Monitors tracking errors (TE) from laser
    * Works in conjuction with the CXA1372AQ over the PF0-2.  
  * Communicates with the CXD2500AQ DSP over the PX0-PX3, PD0 lines
    * Pin 5 / PX0: Serves as the Clock for the Communication
      * Provides Serial Data Clock to the DSP Pin 74 CLOK
    * Pin 6 / PX1: Serial Data Line to DSP Pin 71 DATA
      * The CXP5084 sends data to the CXD2500AQ DSP on this channel
      * The data format is unknown.
    * Pin 7 / PX2: Grounded
    * Pin 8 / PX3: Receives data DSP Pin 66, SQSO
      * Receipts 80-bit Sub Q and 16-bit PCM peak-level data, per the CXD datasheet.
      * Format of the data is unknown.
    * Pin 9 / PD0: Latch for the Serial Data Communication with the DSP
      * Latches at the falling edge.
  * Has an unused COM lines at pins 51-54
    * According to the datasheet, this COM port is attached to the LCD controller/driver
    * It is currently unknown if any data can be extracted from this, or if they are simply disabled.
  * Has unused support for a segmented display.
    * It is currently known if this function is usable with the provided firmare.
    * All segment pins are NC except for the ones shared with the PF bus.
  * Mysteriously connects Pin 26 (NC/No Contact) to +5V even though the data sheet specifically says not to do this.

* Servo: CXA1372AQ
  * Runs in Single Supply Mode off of +5V
    * Reliant on proper function of the midpoint 2.5V "VC" rail
    * If this rail is unstable, it will cause all kinds of odd malfunctions.
  * Receives Serial Data from the CXD2500AQ DSP
    * Pin 24, DATA, receives serial input from Pin 77, DAT0 on the DSP
    * Pin 23, XLT, is used as a latch from XLTO, Pin 78 on the DSP
    * Pin 22, CLK, is used as the 
  * Controls the Focus via the the BA6294 BTL Driver
    * Pin 5, FEO, is connected to Pin 7, FEI on the BTL Driver
    * This adjusts the height of the laser for focusing.
    * Monitors Focus Errors from the Laser via Pin 47, FE
      * This is received via Pin 10 on the Laser, FE
  * Controls the Tracking Diode via the AN8377N Motor Driver
    * Pin 11, TAO, connects to Pin 6 on the AN8377N
    * Monitors Tracking Errors form the Laser via Pin 43, TE
      * This is received from Pin 11 on the Laser, TE
      * This data is shared with the CXP5084 while in route via the PF bus
  * Controls the Sled Drive via the the BA6294 BTL Driver
    * Pins 13-15 control the Sled
      * SLO (Sled Out) connects to Pin 4 on the BTL Driver, which is the input pin.
    * The Sled is stopped when it reaches home position.
      * SSTOP signal on Pin 18 signals stop from S100
  * Handles Eye Pattern Generation
    * RFI->RFO Loop exposes check point for RF Eye Pattern
      * Test point is provided near these pins
      * Additonal Test Point routed to CN4 
  * Signals Defects in the EFM signal to the CXP5084 MCU
    * Outputs on Pin 30, DFCT.
    * Transfers through Q503 transistor to Pin 30 / PE0 on the CXP5084
  * Rolls up Status to the CXD2500AQ DSP
    * Reports the data via Pin 27, SENS
    * Connects to DSP Pin 75, SEIN
    * Data sent upon request from DSP
    * Can Output: FZC, AS, TZC, SSTOP
    * This data gets forwarded to to CXP5084 MCU via DSP Pin 69, SENS    
  * Outputs Track Counter to the CXD2500AQ DSP
    * Output on Pin 26, C.OUT
    * Sent to the DSP Pin 76, CNIN

* DSP: CXD2500AQ
  * Runs on +5V power
    * Does not separate analog/digital power or ground
  * Owns the master board clock
    * 16.3944mhz crystal at X100
    * Passes to clock to main board via Pin 58 (C16M)
      * Signal is EMI Filtered at F100
      * AC coupled at C134
      * Biased to VC at R165
  * Derives 4mhz MCU clock at Pin 57 (C4M)
    * Connected to CXP5084 via R157
    * Wired to CXP5084 Pin 61 (EXTAL)
  * Communicates with the CXA1372AQ 
    * Sends Serial Data
      * Data on Pin 77 (DATO)
      * Latch on Pin 78 (XLTO)
      * Clock on Pin 79 (CLKO)
    * Receives Track Count on Pin 76 (CNIN)
    * Receives Data Bundle on Pin 75 (SEIN)
      * FZC, AS, TZC, SSTOP
    * Receives RF Signal from the CXA1372AQ
      * Input on Pin 24 from Pin 32 of the CXA1372AQ (EFM)
    * Receives FOK from the CXA137AQ on Pin 1 (FOK) when focus is locked.
    * Asserts LOCK if it believes the Sled is running away (Pin 7)
      * Monitors GFS data for 8 repeated low asserts
  * Communicates with the CXP5084 MCU      
    * Receives Serial Data from the CXP5084
      * Data on Pin 71 (DATA)
      * Latch on Pin 72 (XLAT)
      * Clock on Pin 74 (CLOK)
    * Provides data from the CXA1372AQ to the CXP5084 on request
      * Transmitted to the CXP5084 via the SENS Line (Pin 69)
    * Sends SubQ / PCM data to the CXP5084
      * Uses SQSO channel on Pin 66 of the CXD2500AQ
      * Connects to Pin 6 on the CXP5084 (PX1)
  * Controls the Spindle
    * Base signal provided to the L358 on MDP (Pin 4)
    * L358 amped signal connects through to AN8377N's Channel 1 (Pin 5)
  * Provides Audio Data to the LC8951 on the Mainboard
    * 16.9334mhz Clock Output from Pin 58 (C16M)
    * Digtal Output is Disabled
      * Pin 59 (MD2) pulled to ground disables it
      * Pin 60 (DOUT) is thus not connected
        * It is also NC on CN3 (Pin 11)
    * Outputs in Serial Mode
      * Configuration by Pin 30 (PSSL) pulled to ground
      * LRCK Left/Right Channel Clock from Pin 32
      * BCLK Bit Clock from Pin 35
      * WFCK Write Frame Clock from Pin 62
      * DATA from Pin 34
  * Error Correction Reporting (M0-M3)
    * Using a logic analyzer, you can readout the error correction data
    * CD data has two correction codes: C1 and C2
      * C1 has 28 bytes of data and 4 bytes of parity
      * C2 has 24 bytes of data and 4 bytes of parity.
    * Error code table:
      * 0 0 0 0 | C1: No error detected, C1: pointer reset
      * 0 0 0 1 | C1: 1 error corrected, C1: pointer reset
      * 0 0 1 0 | Unknown / Invalid Code
      * 0 0 1 1 | Unknown / Invalid Code
      * 0 1 0 0 | C1: No error detected, C1: pointer set
      * 0 1 0 1 | C1: 1 error corrected, C1: pointer set
      * 0 1 1 0 | C1: 2 errors corrected, C1: pointer set
      * 0 1 1 1 | C1: Uncorrectable error, C1: pointer set
      * 1 0 0 0 | C2: No error detected, C2: pointer reset
      * 1 0 0 1 | C2: 1 error corrected, C2: pointer reset
      * 1 0 1 0 | C2: 2 errors corrected, C2: pointer reset
      * 1 0 1 1 | C2: 3 errors corrected, C2: pointer reset
      * 1 1 0 0 | C1: 4 errors corrected, C2: pointer reset
      * 1 1 0 1 | Unknown / Invalid Code
      * 1 1 1 0 | C2: Uncorrectable error, C1: pointer copied
      * 1 1 1 1 | C2: Uncorrectable error, C1: pointer set
      

* Op-Amp: L358
  * Generates the SPIN signal for the Spindle
    * Seeded from MDP on Pin 4 of the CXD2500AQ
    * Outputs to Channel 1 (Pin 5) of the AN8377N
  * Passes LV (Laser Voltage) to the CXP5084
    * Connects on PC3 (Pin 16)
    * CXP5084 signals LDON (Laser Diode On) via Pin 11 (PD2)

* Motor Driver: AN8377N
  * 3 Channel Motor Driver
    * Uses +9V Power on VCC/PVCC from SVCC Rail
    * +5V Connection to VMON Pin 4
    * +2.5V VC Connection to VREF for Bias
    * Correct operation heavily depends on VC rail being stable.
  * Handles the Spindle on Channel 1
    * Input is generated by the L358 and fed in on Pin 5
    * Derived from MDP on the CXD2500AQ DSP Pin 4
    * Drives the Spindle Motor with Pins 13 & 14
  * Handles the Tracking Diode on Channel 2
    * Input is sourced from CXA1372AQ Pin 11, TAO
    * Drives the Tracking Diode with Pins 11 & 12
    * Connects to Laser Pins 2 & 3
  * Handles the Tray Motor on Channel 3
    * Input is sourced from CXP5084 Pin 17, PB0
    * Drives the Tray Motor using Pins 9 and 10
    * Connects to the Eject Daughterboard via CN2
  * Reset Line Pulled up to SVCC via 22k resistor
  * Pins 3 and 16 are Not Connected

* BTL Driver: BA6294
  * 2 Channel BTL Driver for CD Players
  * Uses 9V Power from SVCC
  * Combo Bias to +5V VCC (R164, 12K) / +2.5V VC (R124, 47K) line on Pin 5
  * Channel 1 Drives the Sled
    * Pin 4 is the Sled Input, received from CXA1372AQ Pin 14
    * Pins 2 and 3 drive the Sled Motor
  * Channel 2 Drives the Focus Diode
    * Drives the distance between the laser diode and the disc
    * Pin 7 is the Focus Input, received from CXA1372AQ Pin 5
    * Pins 8 and 9 Drive the Focus Diode
    * Connects via Pins 1 & 4 on the Laser
  
# License

This documentation is offered under the [CC-BY-SA](https://creativecommons.org/licenses/by-sa/4.0/) license. Linked files may carry their own licenses.
