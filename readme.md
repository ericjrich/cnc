*20240111_1215_est_EJR*

**Eric J. Rich (aka EJR)**
- Email: ericjrich@gmail.com
- Website: [ericjrich.com](https://htmlpreview.github.io/?https://github.com/ericjrich/sites/main/apps/index.html)
- Github: [ericjrich](https://github.com/ericjrich)
---

![a-ypart](a-ypart.gif)

**DOOSAN PUMA TT1800SY USE WITH A Y-AXIS PART-OFF TOOL**

- These programs are used in conjunction to aid users in running their parts on a Doosan Puma TT1800SY lathe with a Y-Axis Part-Off Tool.

- The programs consist of the main programs (Upper and Lower) and several sub-programs. I'll provide an overview of how they work together.

- The main programs (Upper and Lower) serve as templates for user-specific code and contain sections that need to be modified by the users.
  - Sections marked as "MUST BE CHANGED" should be edited according to the specific requirements of the part being machined.
  - Sections marked as "SHOULD CHECK THESE" and "UNLIKELY TO CHANGE" may also need to be reviewed and adjusted if necessary.

- The main programs also include auto-calculated variables and logic checks to ensure safe operation.
  - Auto-calculated variables are determined based on the values provided in the modified sections.
  - Logic checks include conditional statements that validate certain conditions before executing specific actions.

- The sub-programs are called within the main programs and perform specific tasks such as home return, secondary home return, bar change, manual part-off, ejecting and cutoff, and part-off with hand unload.
  - These sub-programs contain additional macros and logic specific to their respective tasks.

---

**What things are reserved for who?**

**Line Numbers (\#):**
```
 N0001-N0999 (Available for Users)
 N1000-N2999 (Reserved For Admin: PartOff)
 N3000-N3999 (Reserved For Admin: Errors)
```
**Wait-Codes (M#):**
```
 M900 (SYNC @ START)
 M901-M948 (Available for Users)
 M949-M960 (Reserved For Admin: PartOff-MAIN)
 M961-M980 (Reserved For Admin: PartOff-SUBS)
```
**Macro Variables:**
```
 #500-#520 (PartOff Related)
 #521-#549 (Reserved For Admin)
 #550-#599 (Available for Users)
```
---

**THE PROGRAMS THAT WE WILL USE TO ACHIEVE OUR GOAL**

**Main Programs:**

  - Please note that "PN" in the file names represents a placeholder for the specific part number or identifier.
  - "REVX" and "OPXX" also represent placeholders for revision numbers and operation numbers, respectively.
  - Users should replace these placeholders with the appropriate values for their specific programs.
  - `O1001(PNTEMPLATE-REVX-OPXX-UPPER)`: Upper Program Template
    - Provides a template for user-specific code for the upper turret of the Doosan Puma TT1800SY lathe. Users can modify sections to customize the program for their specific part.
  - `O1001(PNTEMPLATE-REVX-OPXX-LOWER)`: Lower Program Template
    - Provides a template for user-specific code for the lower turret of the Doosan Puma TT1800SY lathe. Users can modify sections to customize the program for their specific part.

**Sub-Programs:**

  - `O0001(PNHOME-POS-UPPER)`: Home Return - Primary (Upper)
    - Returns the upper turret to the home position. Called at the start or end of a tool operation.
  - `O0001(PNHOME-POS-LOWER)`: Home Return - Primary (Lower)
    - Returns the lower turret to the home position. Called at the start or end of a tool operation.
  - `O0002(PN2ND-HOME-START-UPPER)`: Home Return - Secondary (Upper)
    - Returns the upper turret to the secondary home position. Called at the start of a tool operation.
  - `O0002(PN2ND-HOME-START-LOWER)`: Home Return - Secondary (Lower)
    - Returns the lower turret to the secondary home position. Called at the start of a tool operation.
  - `O0003(PN2ND-HOME-END-UPPER)`: Home Return - Secondary (Upper)
    - Returns the upper turret to the secondary home position. Called at the end of a tool operation.
  - `O0003(PN2ND-HOME-END-LOWER)`: Home Return - Secondary (Lower)
    - Returns the lower turret to the secondary home position. Called at the end of a tool operation.
  - `O8001(PNBAR-CHANGE-UPPER)`: Bar Change (Upper)
    - Performs a bar change operation on the upper turret. Called when changing the bar.
  - `O8002(PNMANUAL-PARTOFF-UPPER)`: Manual Part-Off (Upper)
    - Performs a manual part-off operation on the upper turret. Called for manual part-off processes.
  - `O8003(PNEJECT-AND-CUTOFF-UPPER)`: Part-Off and Ejecting (Upper)
    - Performs part-off and ejecting operation on the upper turret. Called for automated part-off processes.
  - `O8003(PNEJECT-AND-CUTOFF-LOWER)`: Part-Off and Ejecting (Lower)
    - Performs part-off and ejecting operation on the lower turret. Called for automated part-off processes.
  - `O8004(PNEJECT-AND-CUTOFF-UPPER-LPO)`: Part-Off and Ejecting - Last Part Off (Upper)
    - Performs the last part-off and ejecting operation on the upper turret. Called for the last part-off process on a bar.
  - `O8004(PNEJECT-AND-CUTOFF-LOWER-LPO)`: Part-Off and Ejecting - Last Part Off (Lower)
    - Performs the last part-off and ejecting operation on the lower turret. Called for the last part-off process on a bar.
  - `O8013(PNHAND-UNLOAD-AND-CUTOFF-UPPER)`: Part-Off and Hand Unload (Upper)
    - Performs part-off and hand unload operation on the upper turret. Called for part-off processes requiring manual unloading.
  - `O8013(PNHAND-UNLOAD-AND-CUTOFF-LOWER)`: Part-Off and Hand Unload (Lower)
    - Performs part-off and hand unload operation on the lower turret. Called for part-off processes requiring manual unloading.

---

| Variable | Description                          | Usage in Programs                              | +/-         | Modify?       |
|----------|--------------------------------------|-----------------------------------------------|-------------|--------------|
| #515     | Macro Control Variable               | O1001, O1001.P-2                               |             | Leave Alone   |
|          |                                      | Controls the execution and flow of the program |             |              |
|          |                                      | by indicating which macros should be called   |             |              |
|          |                                      | and when.                                     |             |              |
| #509     | LPO-Z-POS (Last Part Off Z Pos)      | O1001, O1001.P-2                               |             | Leave Alone   |
|          |                                      | Specifies the Z-axis position for the last    |             |              |
|          |                                      | part-off on a bar. It ensures the cutoff tool |             |              |
|          |                                      | doesn't collide with the spindle.             |             |              |
| #513     | Stock Stick Out From Main Spindle    | O1001, O1001.P-2                               | +           | Must Be Changed |
|          |                                      | Determines the distance the stock material    |             |              |
|          |                                      | extends beyond the main spindle during        |             |              |
|          |                                      | machining operations.                         |             |              |
| #502     | STOCK:DIA (Stock Diameter)           | O1001, O1001.P-2, O8002, O8004.P-2             | +           | Must Be Changed |
|          |                                      | Represents the diameter of the raw stock      |             |              |
|          |                                      | material from which the part is being         |             |              |
|          |                                      | machined.                                    |             |              |
| #511     | PART:OAL (PART: Over All Length)      | O1001, O1001.P-2                               | +           | Must Be Changed |
|          |                                      | Represents the total length of the part       |             |              |
|          |                                      | being machined, including any allowances or   |             |              |
|          |                                      | protrusions.                                 |             |              |
| #504     | SUB:SWALLOW-POS (Sub-Spld SwlP)      | O1001, O1001.P-2, O8003, O8004                 | -           | Must Be Changed |
|          |                                      | Defines the position at which the sub-spindle |             |              |
|          |                                      | "swallows" or grips the part for cutoff or   |             |              |
|          |                                      | transfer operations.                         |             |              |
| #506     | SUB:JAW-WIDTH (Sub-Spld Jaw W)       | O1001, O1001.P-2, O8004.P-2                    | +           | Must Be Changed |
|          |                                      | Specifies the width of the jaws on the       |             |              |
|          |                                      | sub-spindle chuck used for gripping the part.|             |              |
| #508     | P/O:RPM (Part-Off RPM)               | O1001, O1001.P-2, O8003, O8004                 | +           | Should Be Verified |
|          |                                      | Sets the rotational speed (in revolutions per|             |              |
|          |                                      | minute) of the spindle during the part-off   |             |              |
|          |                                      | operation.                                   |             |              |
| #507     | P/O:FEED-IPR (Part-Off Feed R)       | O1001, O1001.P-2, O8003, O8004                 | +           | Should Be Verified |
|          |                                      | Determines the feed rate in inches per       |             |              |
|          |                                      | revolution for the part-off operation.       |             |              |
| #500     | Y-PartOff: Y Cut To Loc 1of2         | O1001, O1001.P-2, O8003, O8004                 | '0 to +.010  | Unlikely To Change |
|          |                                      | (unless you are doing tubing)                 |             |              |
|          |                                      | Specifies the Y-axis position for the         |             |              |
|          |                                      | beginning of the part-off cut during the     |             |              |
|          |                                      | Y-axis part-off operation.                   |             |              |
| #510     | Y-PartOff: Y Cut To Loc 2of2         | O1001, O1001.P-2, O8003, O8004                 | (- or <= +.010")| Unlikely To Change |
|          |                                      | Specifies the Y-axis position for the end of |             |              |
|          |                                      | the part-off cut during the Y-axis part-off |             |              |
|          |                                      | operation.                                   |             |              |
| #501     | PART:FCE-AMT (Part Face Amnt)        | O1001, O1001.P-2                              |             | Almost Never Changed |
|          |                                      | Specifies the amount of material to be       |             |              |
|          |                                      | removed from the face of the part during the |             |              |
|          |                                      | part-off operation.                          |             |              |
| #512     | P/0:TOOL-WIDTH (Part-Off T W)        | O1001, O1001.P-2                              |             | Almost Never Changed |
|          |                                      | Indicates the width of the part-off tool     |             |              |
|          |                                      | used for cutting off the part from the stock |             |              |
|          |                                      | material.                                   |             |              |
| #503     | A-RAPID-POS (A-Axis Rapid Pos)       | O1001, O1001.P-2                              |             | Almost Never Changed |
|          |                                      | Determines the rapid movement position of    |             |              |
|          |                                      | the A-axis (rotary axis) during specific    |             |              |
|          |                                      | operations.                                  |             |              |
| #514     | Last Part Off Sanity Check           | O1001, O1001.P-2                              |             | Leave Alone   |
|          |                                      | Verifies the minimum distance between the    |             |              |
|          |                                      | part-off tool zero and the spindle face to  |             |              |
|          |                                      | prevent collision during the last part-off  |             |              |
|          |                                      | operation.                                  |             |              |

---

#THE MAIN PROGRAMS (TEMPLATES) FOR UPPER AND FOR LOWER
```
%
O1001(PNTEMPLATE-REVX-OPXX-UPPER)
(FILE: O1001)
(----DOOSAN PUMA TT1800SY w/ Y-PartOff----)
(Upper Program)
(Template For Your Code)
(-----------------------------------------)
(20230603)(1830 est)(EJR)


#515=0(*LEAVE ALONE*)
(#515=0 should be before N1234)
N1234(-----MACRO-AREA------)


(========MUST BE CHANGED========)
#513=0.001(+)(Stock Stick Out From Main Spidle)
#502=6.0(+)(STOCK:DIA)
#511=0.001(+)(PART:OAL or Over All Length)
#504=-0.001(-)(SUB:SWALLOW-POS or how far the 2nd spindle goes over the part it is grabbing for the cutoff/transfer)
#506=3.0(+)(Width of the 2nd spindles jaws from the face of its chuck)

(=SHOULD CHECK THESE=)
#508=1200(+)(PartOff RPM)
#507=.0040(+)(PartOff FeedRate Inches Per Rev or IPR)

(=UNLIKELY TO CHANGE=)
#500=-0.0750(-)(Y-PartOff: Y Cut To Location 1of2 goes at feed rate #507)
#510=0.0100(- or <= +.010")(Y-PartOff: Y Cut To Location 2of2 goes at feed rate .001)

(=ALMOST NEVER CHANGED=)
#501=0.0200(+)(PART:FCE-AMT)
#512=0.1180(+)(P/0:TOOL-WIDTH)
#503=0.1000(+)(A-RAPID-POS)

(===============================)
(========EDIT ABOVE HERE========)
(===============================)

(--AUTO-CALC--)(*LEAVE ALONE*)
#509=[0-#511-#501-#512](Z Cutoff Position when running LastPartOff For that bar / it is closer becuase it isn't pulling the bar for the next part)
#505=[#511+#504+#512+#501+#501](A-Axis-PULL-Position when doing a normal PartOff Cut/Transfer)
#514=[#513+#509](Last Part Off Sanity Check to ensure that we wont collide the partoff with the spindle)

(--LOGIC--)(*LEAVE ALONE*)
IF[#514LE.1]GOTO3000(if there isn't at least .100" between the partoff tool zero and the spindle face it will not let you make that mistake)
(-----------------------)
M1
(--MACRO-FETCH-RETURN--)(*LEAVE ALONE*)
(these are used during the cutoff area of the program to ensure the macros are called again and returns the program to the correct location after reading macros)
IF[#515EQ1]GOTO1002 (returns to regular PartOff)
IF[#515EQ2]GOTO2002 (returns to regular Last Part On Bar AKA LPO or LastPartOff)
(-----------------------)
G20(M12)
M98P1
M900(SYNC-UP)
()
(===============================)
(========PART-CODE-BELOW========)
(===============================)


( USERS CAN USE: )
(-)(N0001-N0999)
(-)(M901-M948)
(-)(#550-#599)

(=========DONT-EDIT-BELOW=========)

(*)M949(*)
M05P11(S1 STOP)
M05P12(UT STOP)
M9(COOLANT OFF)
M34(MILL-MODE-OFF)
M98P1(HOME)
G18(XZ PLANE)
(*)M950(*)
(------)
/M00
N1000(EJECT, P/O, TRANSFER)
M98P1
/2GOTO2000
N1001(TRANSFER)

#515=1(RETURN-MACRO)
GOTO1234(FETCH-MACROS)
N1002(FETCH-DONE)

(*)M951(*)
M98P8003(SUB)
(*)M952(*)
#515=0
M30

(====LPO-BELOW====)

N2000(EJECT, P/O, TRANSFER-LPO)
N2001(TRANSFER)

#515=2(RETURN-MACRO)
GOTO1234(FETCH-MACROS)
N2002(FETCH-DONE)

(*)M953(*)
M98P8004(SUB-LPO)
(*)M954(*)
/2M98P8001(BAR CHANGE)
(*)M960(*)
#515=0
M30

N3000(ERROR)
#515=0
M0
(LPO TOO CLOSE TO H1)
M2
%
```
---
```
%
O1001(PNTEMPLATE-REVX-OPXX-LOWER)
(FILE: O1001.P-2)
(----DOOSAN PUMA TT1800SY w/ Y-PartOff----)
(Lower Program)
(Template For Your Code)
(-----------------------------------------)
(20230603)(1830 est)(EJR)


#515=0(*LEAVE ALONE*)
(#515=0 should be before N1234)
N1234(-----MACRO-AREA------)

(===============================)
(========MUST BE CHANGED========)
(===============================)

#513=0.001(+)(Stock Stick Out From Main Spidle)
#502=6.0(+)(STOCK:DIA)
#511=0.001(+)(PART:OAL or Over All Length)
#504=-0.001(-)(SUB:SWALLOW-POS or how far the 2nd spindle goes over the part it is grabbing for the cutoff/transfer)
#506=3.0(+)(Width of the 2nd spindles jaws from the face of its chuck)

(=SHOULD CHECK THESE=)
#508=1200(+)(PartOff RPM)
#507=.0040(+)(PartOff FeedRate Inches Per Rev or IPR)

(=UNLIKELY TO CHANGE=)
#500=-0.0750(-)(Y-PartOff: Y Cut To Location 1of2 goes at feed rate #507)
#510=0.0100(- or <= +.010")(Y-PartOff: Y Cut To Location 2of2 goes at feed rate .001)

(=ALMOST NEVER CHANGED=)
#501=0.0200(+)(PART:FCE-AMT)
#512=0.1180(+)(P/0:TOOL-WIDTH)
#503=0.1000(+)(A-RAPID-POS)

(===============================)
(========EDIT ABOVE HERE========)
(===============================)

(--AUTO-CALC--)(*LEAVE ALONE*)
#509=[0-#511-#501-#512](Z Cutoff Position when running LastPartOff For that bar / it is closer becuase it isn't pulling the bar for the next part)
#505=[#511+#504+#512+#501+#501](A-Axis-PULL-Position when doing a normal PartOff Cut/Transfer)
#514=[#513+#509](Last Part Off Sanity Check to ensure that we wont collide the partoff with the spindle)

(--LOGIC--)(*LEAVE ALONE*)
IF[#514LE.1]GOTO3000(if there isn't at least .100" between the partoff tool zero and the spindle face it will not let you make that mistake)
(-----------------------)
M1
(--MACRO-FETCH-RETURN--)(*LEAVE ALONE*)
(these are used during the cutoff area of the program to ensure the macros are called again and returns the program to the correct location after reading macros)
IF[#515EQ1]GOTO1002 (returns to regular PartOff)
IF[#515EQ2]GOTO2002 (returns to regular Last Part On Bar AKA LPO or LastPartOff)
(-----------------------)

G20(M12)
M98P1
M900(SYNC-UP)
()
(===============================)
(========PART-CODE-BELOW========)
(===============================)

( USERS CAN USE: )
(-)(N0001-N0999)
(-)(M901-M948)
(-)(#550-#599)

(=========DONT-EDIT-BELOW=========)

(*)M949(*)
M05P21(S2 STOP)
M05P22(LT STOP)
M9(COOLANT OFF)
M134(MILL-MODE-OFF)
M98P1(HOME)
G18(XZ PLANE)
(*)M950(*)
(------)
/M00
N1000(EJECT, P/O, TRANSFER)
M98P1
/2GOTO2000
N1001(TRANSFER)

#515=1(RETURN-MACRO)
GOTO1234(FETCH-MACROS)
N1002(FETCH-DONE)

(*)M951(*)
M98P8003(SUB)
(*)M952(*)
#515=0
M30

(====LPO-BELOW====)

N2000(EJECT, P/O, TRANSFER-LPO)
N2001(TRANSFER)

#515=2(RETURN-MACRO)
GOTO1234(FETCH-MACROS)
N2002(FETCH-DONE)

(*)M953(*)
M98P8004(SUB-LPO)
(*)M954(*)
(*WAIT FOR BAR CHNG*)
(*)M960(*)
#515=0
M30

N3000(ERROR)
#515=0
M0
(LPO TOO CLOSE TO H1)
M2
%
```
---
#THE SUB PROGRAMS WE REFERNCED ABOVE#


```
%
O0001(PNHOME-POS-UPPER)
(FILE: O0001)
(----DOOSAN PUMA TT1800SY w/ Y-PartOff----)
(Upper Program)
(Home Return: Primary)
(Called At: Start/End Of Tool)
(-----------------------------------------)
(20230603)(1830 est)(EJR)

G99G80G40
G28V0U0
G28W0
M99
%
```
---
```
%
O0001(PNHOME-POS-LOWER)
(FILE: O0001.P-2)
(----DOOSAN PUMA TT1800SY w/ Y-PartOff----)
(Lower Program)
(Home Return: Primary)
(Called At: Start/End Of Tool)
(-----------------------------------------)
(20230603)(1830 est)(EJR)

G99G80G40
G28U0
G28W0
M99
%
```
---
```
%
O0002(PN2ND-HOME-START-UPPER)
(FILE: O0002)
(----DOOSAN PUMA TT1800SY w/ Y-PartOff----)
(Upper Program)
(Home Return: Secondary)
(Called At: Start Of Tool)
(-----------------------------------------)
(20230603)(1830 est)(EJR)

G99G80G40
G30V0U0W0
M56
M99
%
```
---
```
%
O0002(PN2ND-HOME-START-LOWER)
(FILE: O0002.P-2)
(----DOOSAN PUMA TT1800SY w/ Y-PartOff----)
(Lower Program)
(Home Return: Secondary)
(Called At: Start Of Tool)
(-----------------------------------------)
(20230603)(1830 est)(EJR)

G99G80G40
G30U0W0
M56
M99
%
```
---
```
%
O0003(PN2ND-HOME-END-UPPER)
(FILE: O0003)
(----DOOSAN PUMA TT1800SY w/ Y-PartOff----)
(Upper Program)
(Home Return: Secondary)
(Called At: End Of Tool)
(-----------------------------------------)
(20230603)(1830 est)(EJR)

G99G80G40
G30V0U0W0
M57
M99
%
```
---
```
%
O0003(PN2ND-HOME-END-LOWER)
(FILE: O0003.P-2)
(----DOOSAN PUMA TT1800SY w/ Y-PartOff----)
(Lower Program)
(Home Return: Secondary)
(Called At: End Of Tool)
(-----------------------------------------)
(20230603)(1830 est)(EJR)

G99G80G40
G30U0W0
M57
M99
%
```
---
```
%
O8001(PNBAR-CHANGE-UPPER)
(FILE: O8001)
(----DOOSAN PUMA TT1800SY w/ Y-PartOff----)
(Upper Program)
(Bar Change)
(THERE IS NO LOWER VERSION)
(-----------------------------------------)
(20230603)(1830 est)(EJR)

(----MACRO-NOTES----)
(#502=STOCK:DIA)
(#501=PART:FCE-AMT)
(#508=P/O:RPM)
(#507=P/O:FEED-IPR)
(------------------)
N1
G54
G28U0
G28W0
M9
M5P11
T1212G54
M19S350
G4X2.
M10(PTC UP)
M69
M51(BARFEED COMMAND)
M68
G4U2.0
M11(PTC DOWN)
M1

N2(TOP CUT)
M5P21
M5P11
G99
G0G54T1212
M34
G0G54Z#501
X[#502+.050]Y[-.05-[#502/2]]M7
X0.0
G4U6.0(TEST)
G97S[#508]M203P11
G1G99Y-.075F[#507]
Y.01F.001
G0G54W.01
X[#502+.050]
G28U0V0
M9
M111(INTF CHECK ON)
M1
M99
%
```
---
```
%
O8002(PNMANUAL-PARTOFF-UPPER)
(FILE: O8002)
(----DOOSAN PUMA TT1800SY w/ Y-PartOff----)
(Upper Program)
(Manual-Part-Off)
(THERE IS NO LOWER VERSION)
(-----------------------------------------)
(20230603)(1830 est)(EJR)

(MAKE SURE YOUR MACROS WERE)
(ALREADY CALLED)
(----MACRO-NOTES----)
(#502=STOCK:DIA)
(#501=PART:FCE-AMT)
(#508=P/O:RPM)
(#507=P/O:FEED-IPR)
(------------------)
N1M98P1
M7
M34
G0G54T1212
G0G54Z#501
(.......................)
G0X[#502+.1]Y-.2
G0X[#502*.9659+.1]Y[#502*[-.1913]-.05]
G0X[#502*.7071+.1]Y[#502*[-.3536]-.05]
G0X[#502*.5000+.1]Y[#502*[-.4619]-.05]
G0X0.0Y[#502*[-.5]-.05]
(........................)
G97S[#508]M3P11
G1G99Y-.075F[#507]
Y.01F.001
G0W.02
X[#502+.050]
M98P1
M9
M2
%
```
---
```
%
O8003(PNEJECT-AND-CUTOFF-UPPER)
(FILE: O8003)
(----DOOSAN PUMA TT1800SY w/ Y-PartOff----)
(Upper Program)
(Part-Off And Ejecting)
(-----------------------------------------)
(20230603)(1830 est)(EJR)


(----MACRO-NOTES----)
(#502=STOCK:DIA)
(#501=PART:FCE-AMT)
(#500=P/O:Y-#1)
(#510=P.O:Y-#2)
(#508=P/O:RPM)
(#507=P/O:FEED-IPR)
(------------------)
N1(EJECT)
M7
(*)M961(*)
M34
M5P11
M1
G28V0
G28U0
G28W0(SAFETY POSITION)
T1212
G53Z6.0
(*)M962(*)
(*WAITING*)
(*)M963(*)
M110
G0G54Z#501(->)
(.......................)
G0X[#502+.1]Y-.2
G0X[#502*.9659+.1]Y[#502*[-.1913]-.05]
G0X[#502*.7071+.1]Y[#502*[-.3536]-.05]
G0X[#502*.5000+.1]Y[#502*[-.4619]-.05]
G0X0.0Y[#502*[-.5]-.05]
(........................)
G50S3000
G97S[#508]M203P11
G1G99Y[#500]F[#507]
Y[#510]F.001
(*)M964(*)
(*WAITING*)
(*)M965(*)
G0G54W.003
G28U0V0
(*)M966(*)
M98P1
M9
(*)M967(*)
M99
%
```
---
```
%
O8003(PNEJECT-AND-CUTOFF-LOWER)
(FILE: O8003.P-2)
(----DOOSAN PUMA TT1800SY w/ Y-PartOff----)
(Lower Program)
(Part-Off And Ejecting)
(-----------------------------------------)
(20230603)(1830 est)(EJR)


(----MACRO-NOTES----)
(#506=SUB:JAW-WIDTH)
(#504=SUB:SWALLOW-POS)
(#503=A-RAPID-POS)
(#505=A-PULL-POS)
(------------------)
N1
(EJECT)
M114
(*)M961(*)
M134
M5P21
G28U0.
G28W0.
M1

G0G53Z7.M310
M115
(*)M962(*)
G4U.2
M288M108M194
G4U.2
G53A[-6.2+#506]
M131
M169
M114
M116
M115
M12
G4U.5
M114M14
M131
G55
N12
M192M169
M194M110
G0G59A[#503+2]G97S200(S500)M213P11
G4U.5(PAUSE)
M115
M109(THRU-COOL-OFF)
G0G59A#503
G1G98A#504F200.
M168
M15
G4U.5
M31
M69
G4U.5
G1A#505F200.
(*)M963(*)
M68
(*)M964(*)
G0G59A[#505+.006](->)
(*)M965(*)
G55
G28A0.0
(*)M966(*)
G28W0.M195M265
M9
(*)M967(*)
M99
%
```
---
```
%
O8004(PNEJECT-AND-CUTOFF-UPPER-LPO)
(FILE: O8004)
(----DOOSAN PUMA TT1800SY w/ Y-PartOff----)
(Upper Program)
(Part-Off: Last Part Off For This Bar)
(-----------------------------------------)
(20230603)(1830 est)(EJR)

(----MACRO-NOTES----)
(#502=STOCK:DIA)
(#500=P/O:Y-#1)
(#510=P.O:Y-#2)
(#508=P/O:RPM)
(#507=P/O:FEED-IPR)
(#509=LPO-Z-POS)
(------------------)
N1(EJECT)
M7
(*)M961(*)
M34
M5P11
M1
G28V0
G28U0
G28W0(SAFETY POSITION)
T1212
G53Z6.0
(*)M962(*)
(*WAITING*)
(*)M963(*)
M110
G0G54Z#509(->)

(.......................)
G0X[#502+.1]Y-.2
G0X[#502*.9659+.1]Y[#502*[-.1913]-.05]
G0X[#502*.7071+.1]Y[#502*[-.3536]-.05]
G0X[#502*.5000+.1]Y[#502*[-.4619]-.05]
G0X0.0Y[#502*[-.5]-.05]
(........................)
G50S3000
G97S[#508]M203P11
G1G99Y[#500]F[#507]
Y[#510]F.001
(*)M964(*)
(*WAITING*)
(*)M965(*)
G0G54W.003
G28U0V0
(*)M966(*)
M98P1
M9
(*)M967(*)
M99
%
```
---
```
%
O8004(PNEJECT-AND-CUTOFF-LOWER-LPO)
(FILE: O8004.P-2)
(----DOOSAN PUMA TT1800SY w/ Y-PartOff----)
(Lower Program)
(Part-Off: Last Part Off For This Bar)
(-----------------------------------------)
(20230603)(1830 est)(EJR)


(----MACRO-NOTES----)
(#506=SUB:JAW-WIDTH)
(#504=SUB:SWALLOW-POS)
(#503=A-RAPID-POS)
(------------------)
N1(EJECT)
M114
(*)M961(*)
M134
M5P21
G28U0.
G28W0.
M1

G0G53Z7.M310
M115
(*)M962(*)
M288M108M194
G53A[-6.2+#506]
M131
M169
M114
M116
M115
M12
G4U2.
M114M14
M131
G55
N12
M192M169
M194M110
G0G59A[#503+2]G97S200(S500)M213P11
G4U2.0(PAUSE)
M115
M109(THRU-COOL-OFF)
G0G59A#503
G1G98A#504F200.
M168
M15
G4U.5
(*)M963(*)
(*)M964(*)
G0G59A[#504+.006](->)
(*)M965(*)
G55
G28A0.0
(*)M966(*)
G28W0.M195M265
M9
(*)M967(*)
M99
%
```
---
```
%
O8013(PNHAND-UNLOAD-AND-CUTOFF-UPPER)
(FILE: O8013)
(----DOOSAN PUMA TT1800SY w/ Y-PartOff----)
(Upper Program)
(Part-Off And Hand Unload)
(-----------------------------------------)
(20230603)(1830 est)(EJR)


(----MACRO-NOTES----)
(#502=STOCK:DIA)
(#501=PART:FCE-AMT)
(#500=P/O:Y-#1)
(#510=P.O:Y-#2)
(#508=P/O:RPM)
(#507=P/O:FEED-IPR)
(------------------)
N1(EJECT)
(*)M961(*)
M34
M5P11
M1
G28V0
G28U0
G28W0(SAFETY POSITION)
T1212
G53Z6.0
(*)M962(*)
(*WAITING*)
(*)M963(*)
M110
G0G54Z#501(->)
X[#502+.050]Y[-.05-[#502/2]]M7
X0.0
G50S3000
G97S[#508]M203P11
G1G99Y[#500]F[#507]
Y[#510]F.001
(*)M964(*)
(*WAITING*)
(*)M965(*)
G0G54W.003
G28U0V0
(*)M966(*)
M98P1
M9
(*)M967(*)
M99
%
```
---
```
%
O8013(PNHAND-UNLOAD-AND-CUTOFF-LOWER)
(FILE: O8013.P-2)
(----DOOSAN PUMA TT1800SY w/ Y-PartOff----)
(Lower Program)
(Part-Off And Hand Unload)
(-----------------------------------------)
(20230603)(1830 est)(EJR)


(----MACRO-NOTES----)
(#506=SUB:JAW-WIDTH)
(#504=SUB:SWALLOW-POS)
(#503=A-RAPID-POS)
(#505=A-PULL-POS)
(------------------)
N1(EJECT)
M114
(*)M961(*)
M114
M134
M5P21
G28U0.
G28W0.
M1

G0G53Z7.(M310)
M115
(*)M962(*)
G4U.2
(M288)
M108M194
G4U.2
G53A[-6.2+#506]
M131
M169
M114
M116
M115
(M12)
G4U2.
M114M14
M131
G55
N12
M192M169
M194M110
G0G59A[#503+2]G97S200(S500)M213P11
G4U2.0(PAUSE)
M115
M109(THRU-COOL-OFF)
G0G59A#503
G1G98A#504F200.
M168
M15
G4U.5
M31
M69
G4U.5
G1A#505F200.
(*)M963(*)
M68
(*)M964(*)
G0G59A[#505+.006](->)
(*)M965(*)
G55
G28A0.0
(*)M966(*)
G28W0.M195M265
M9
(*)M967(*)
M99
%
```
---
```
%
O8014(PNHAND-UNLOAD-AND-CUTOFF-UPPER-LPO)
(FILE: O8014)
(----DOOSAN PUMA TT1800SY w/ Y-PartOff----)
(Upper Program)
(Part-Off And Hand Unload: Last Part Off For This Bar)
(-----------------------------------------)
(20230603)(1830 est)(EJR)


(----MACRO-NOTES----)
(#502=STOCK:DIA)
(#500=P/O:Y-#1)
(#510=P.O:Y-#2)
(#508=P/O:RPM)
(#507=P/O:FEED-IPR)
(#509=LPO-Z-POS)
(------------------)
N1(EJECT)
(*)M961(*)
M34
M5P11
M1
G28V0
G28U0
G28W0(SAFETY POSITION)
T1212
G53Z6.0
(*)M962(*)
(*WAITING*)
(*)M963(*)
M110
G0G54Z#509(->)
X[#502+.050]Y[-.05-[#502/2]]M7
X0.0
G50S3000
G97S[#508]M203P11
G1G99Y[#500]F[#507]
Y[#510]F.001
(*)M964(*)
(*WAITING*)
(*)M965(*)
G0G54W.003
G28U0V0
(*)M966(*)
M98P1
M9
(*)M967(*)
M99
%
```
---
```
%
O8014(PNHAND-UNLOAD-AND-CUTOFF-LOWER-LPO)
(FILE: O8014.P-2)
(----DOOSAN PUMA TT1800SY w/ Y-PartOff----)
(Lower Program)
(Part-Off And Hand Unload: Last Part Off For This Bar)
(-----------------------------------------)
(20230603)(1830 est)(EJR)


(----MACRO-NOTES----)
(#506=SUB:JAW-WIDTH)
(#504=SUB:SWALLOW-POS)
(#503=A-RAPID-POS)
(------------------)
N1(EJECT)
M114
(*)M961(*)
M134
M5P21
G28U0.
G28W0.
M1

G0G53Z7.(M310)
M115
(*)M962(*)
(M288)
M108M194
G53A[-6.2+#506]
M131
M169
M114
M116
M115
(M12)
G4U2.
M114M14
M131
G55
N12
M192M169
M194M110
G0G59A[#503+2]G97S200(S500)M213P11
G4U2.0(PAUSE)
M115
M109(THRU-COOL-OFF)
G0G59A#503
G1G98A#504F200.
M168
M15
G4U.5
(*)M963(*)
(*)M964(*)
G0G59A[#504+.006](->)
(*)M965(*)
G55
G28A0.0
(*)M966(*)
G28W0.M195M265
M9
(*)M967(*)
M99
%
```
---
