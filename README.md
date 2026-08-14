# ReFalcon-030
Atari Falcon 030 motherboard replica

1. Preface. 
The ReFalcon motherboard is a replica of Falcon 030 motherboard with the following modifications:
* replaced originally used but obsolete today YM3439 Yamaha synthesizer chip with equivalent YM2149 in DIP package;
* improved COMBEL system clock distribution circuit by adding low propagation clock splitter U701 along with 50 ohm controlled impedance routing to ensure lowest attenuation and highest signal capacity of clock signal path; two additional copper layers were added to the PCB stack to enable continuous controlled impedance routing; 
* the RF modulator was removed enabling a placeholder for analog VGA as well as digital video output, like DVI or HDMI-compatible using pluggable add-ons. A key to this additional is newly added connector J703 providing VIDEL digital R, G and B color digital signals, both synchronization signals and pixel clock signal - all you need to design fully functional video scaler/deinterlacer. I encourage community members to develop such add-on.
* with the original DSP chip becoming hard to find, an optional use of alternate DSP processors was introduced including direct replacement 56001 in widely available QFP-132 package as well experimental use of 56002 DSP derivative through small separate mezzanine PCB which is a carrier for new DSP allowing it to be plugged in to the ReFalcon PCB with original U38 not present (not populated) via P701 through P704 connector headers; a JP8 was also added to separate SDMA clock from the DSP enabling use of higher clock sources for the DSP; the mezzanine PCB is optional and not a part of this repository
* the remaining Falcon 030 circuits remain as is, however all original ECO changes described in Falcon030 Field Service Guide (FSG) were added to the replica. 

2. Published revision.
The uploaded documentation describes ReFalcon motherboard rev 1.1, electronic circuit and printed circuit board.
This version is direct derivative of original rev.1 which was validated by fabrication of fully functional prototype put into series of tests to verify hardware functionality. During these test, a handful of bugs were discovered in the hardware circuit and addressed/fixed resulting in rev 1.1. These fixes were added to the rev.1 and validated. 
The current rev 1.1 represents the rev.1 with the following changes/fixes to eliminate discovered in rev.1 bugs and bring board to desired performance:
* fixed J6-pin48 tie to net CD (rev.1 had this connected to CDATA0 net)
* J6-pin38 separated from net IO (rev.1 had this pin connected to IO net)
* VEE is separated from VEE2 (rev.1 had these two nets connected)
* C6 & C47 polarity marks fixed (rev.1 had these reversed)
* floppy drive spacers/posts mounting holes diameter were adjusted to enable using original plastic and bronze stand offs
One improvement:
* added JP8 jumper for flexibility of driving DSP using external clock source, other than SDMA
NOTE: The rev 1.1 has not been fabricated as of today, although the designer is working on completing the rev 1.1 validation which will be announced here once completed. The risk of fabricating rev 1.1 with all incorporated bug fixes from rev.1 is low, but designer cannot guarantee such risk to be zero. It is recommended to wait with rev 1.1 fabrication until designer confirms it is free of any further issues.

3. Circuit.
The ReFalcon PCB uses 8-layer board stack-up. The one additional signal layer was necessary to properly implement COMBEL clock signal path distribution. An 8th layer - ground plane was added to complement the clock layer (7th) to make the board stack-up symmetrical which makes the PCB fabrication cheaper among most vendors. Also, the addition of extra GND plane (8th layer) allowed sandwiching clock layer in between two ground planes which furtherly improves signal integrity and reduces board EMI footprint with present perimeter board stitching; 
Almost entire original PCB artwork was rerouted in ReFalcon to reduce Gerber's weight and improve design margins for:
* integrated circuits supply decoupling (multiple chips VCC pins found to be missing decoupling capacitor or found to have one remote or with remote GND tie which in both cases greatly reduce decoupling effectiveness). 
* routing clearance (violated in many places on original board) to address component obsolescence and improve PCBA manufacturability;
* soldermask clearance to improve error free automated board assembly;
There is one portion of the circuit around U26D gate present in original FSG schematic (both versions) which is not implemented in the physical Falcon 030 PCB, at least in UK board version. Where the U26D is not connected to any circuit in physical board. The ReFalcon replica adds handful of SMT jumpers to connect this gate to mimic the FSG schematic for experimentation or keep it disconnected as in physical UK PCBA. These jumpers are: JP1 through JP6 and they are present on the bottom side of the PCB. The bill of materials (BOM) lists which jumpers to be shorted and which kept open to bring up the replica as in rev.1 validated prototypes.

4. Bill of materials.
The original BOM printed in FSG was found to have multiple typos. These parts were verified (de-soldered and measured) using physical original Falcon030 UK motherboard and correct component values were added to the replica BOM.
I crossed majority of the obsolete parts with modern equivalents available from mainstream vendors like Digikey or Mouser. Remaining parts are either obsolete chips (DSP, CPU, SCSI controller, Audio Coded, video Coded, Floppy controller, serial controller, etc) are to be found 2nd hand from sources like eBay. Finally the Atari custom chips (Videl, Combel and SDMA) can be still obtainedd from couple US vendors in very limited quantities, or extracted from original Falcon PCBA.

5. Assembly.
The ReFalcon board has over 820 components. It is very complex board to assemble so it is not recommended task for manual-only assembly even for skilled technicians. With over 500 parts to solder just on the bottom side, the rev.1 prototype FABs were ordered with all these components assembled in the factory. Hands soldering these parts is not recommended as risk of error is very high and it takes sometimes one component to fail the board boot. Finding the problem later among that many parts is daunting to impossible job.
The replica allows socketing all TH DIP and PLCC chips including not-socketed in original PCB: U7, U13, U!5, U20, U24, U28, U29, U31, U52. I recommend to socket these to enable replacement or improve board bring up and debugging.
TIP: I strongly suggest using only listed in the BOM parts or direct equivalents. Deviating from listed parts especially for discrete components but also integrated chips, especially PLD chips: U62, U63, U67, U68 where use of listed 7ns versions improves design margin and problem-free board bring up and boot. Original PALs will also work fine. 

6. Bring up.
Ensure, all components are assembled while all DNP marked parts are not present.
Ensure jumpers U46, U47 and JP1 through JP6 are properly configured: U46:1,4 shorted; U47:7 shorted, others open;
Keep JP7 and JP8 open. 
Insert verified working Falcon 030 RAM module
Ensure presence of the W1 jumper on the J20 header. 
Correctly assembled replica PCB should boot with no adjustments assuming application of proper regulated and stabilized quality DC power supply providing
* 5.0V +5%/-0% @ 3.0A minimum
* 12V (+/-5%) @ 1A (required for floppy drive, analog audio in/out, internal speaker)
presence of properly configured and flashed in U51 EMuTOS or original TOS 4.04 if you are replacing damaged Falcon030 motherboard with replica.
The J702 header footprint which connects standard Dsub15HD video socket used with most VGA monitors, can be patched to J5 pins eliminating a need to use of RGB-Dsub23 to VGA adapter. It also helps eliminating a need for DB19 RGB connector which is unobtanium today, and use of Dsub15HD VGA connector instead.
TIP: When using replica PCBA in original Falcon motherboard, install vertical flavor of the VGA connector for J701 to avoid cutting D-hole in the original Falcon enclosure, unless you do not care about keeping Falcon case intact. Fish the cable via existing RF modulator hole (requires rework on the one side of the VGA cable plug).

You most likely going to find many tips for successful assembly and bring up on dedicated to ReFalcon Discord server - ReFalcon030. 

Thank you, 
Steve Suavek
______________________
File closed: 08/08/2026






