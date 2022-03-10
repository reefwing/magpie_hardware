# Magpie Drone DS3 Construction
 PCB Design and BOM for the Magpie Drone.

 ## Overview

 The Nexgen team is designing their own drone to be used for school incursion courses with a duration of 1–3 days. The drone will be controlled by an Arduino Nano 33 BLE or an Arduino Portenta H7 (i.e. the Mbed OS based Arduino cores).

There is some additional hardware and interfacing requirements which we have spoken about in [part 2 of our article on Writing your own Flight Controller Software](https://reefwing.medium.com/how-to-write-your-own-flight-controller-software-part-2-bc7f27214bb2). Our Power Distribution Board (PDB), makes it easy to include these in our drone build. 

Note that there are two different PDB’s for the Nano 33 BLE and the Portenta H7 because they use different header configurations (Portenta uses the MKR form factor and the Nano uses the Nano form factor). This repository describes the construction of the Nano 33 BLE version.

The Power Distribution Board includes the following:

- The provision of a regulated 5VDC from the LiPo battery. The auxillary connectors are connected to this 5V bus when the left two pins of the 5V AUX header is bridged.
- Driving circuitry for the on-board passive piezo buzzer.
- Inverting and level shifting SBUS interface for the X8R remote control transceiver.
- Power indication (LED’s) for battery and 5V.
- Voltage divider to measure the battery voltage.
- Nano 33 BLE external reset.
- Interface points for the X8R SBUS, RSSI and Telemetry, LiPo (XT-60), and ESC power and control.

## Construction

Check the Printed Circuit Board for any obvious defects, then check that you have all the parts you need before firing up the soldering iron.

Start by soldering the low profile components, resistors, transistors, LED’s, JST-SH 2 pin connectors (AUX 5VDC) and piezo buzzer. If the resisitors get mixed up, use a multimeter to make sure that you are mounting the right component. All the part values are screen printed on the PCB. The correct transistor orientation is marked on the PCB.

Double check that you have the correct model voltage regulator. You should be able to read on the part LM1084–5V. The LM1084-ADJ part wont work in this circuit design and we received these when we ordered the 5V model. It took a while to work out what the problem was as the parts look identical.

Mount the regulator and capacitors next. It is easier to do this before the Nano stackable headers are in place. As the Nano sits on top of these components, the regulator needs to be mounted flat (use an M3 nut and bolt to secure it in place before soldering) and the capacitors need to be pushed all the way in (or laid flat). There should be just enough room, but check this before soldering the capacitors.

The correct orientation for the LM1084 is with the heatsink hard against the mounting hole. The heatsink should be touching the PCB. The tantalum capacitors are also polarised. Normally the positive pins are marked and these should match up with the “+” marked pin on the PCB.

We used 0.1' Header Pins in lieu of switches for the 5V AUX and RESET. If you want the 5V bus available you need to bridge the left 2 pins on the 5V AUX. You will need 5V for the X8R remote control transceiver as a minimum so you could just use a wire bridge on those two pins instead of using 0.1' Header Pins. If you do use the header pins, I placed some sticky tape over the top to hold them in place. Solder just one pin and then you can adjust if the part isn’t straight.

Finally, mount the Nano stackable headers. The easiest way to keep these in the correct location is to place them in the PCB and mount the Nano 33 BLE. Then solder one pin on each side and double check that everything looks ok. You can then solder the rest of the pins. The Nano header pins are relatively far apart (compared to the Portenta H7 at least), but it is always better to use less solder and add more if required.

Congratulations! You have completed construction of the Magpie Drone Power Distribution board.

## Testing

Remove the Nano 33 BLE if you have it in place. Connect up the LiPo that you plan to use with your drone. For our prototype we were using a 1300 mAh, 2S (nominal 7.4V), battery. The size of the battery that you use is constrained by two things:

- The Nano 33 BLE: which can handle 5–21V input (Vin); and
- The LM1084–5: 6.5–25V input.

Consequently, our minimum operating voltage is 6.5V and our maximum is 21V.
An individual LiPo cell has a nominal voltage of 3.7V but when fully charged can be around 4.3V although it will quickly drop to 3.7V under normal use. When depleted, the cell will be around 3V.

From this we can determine that the range of suitable LiPo battery serial configurations is 2S (i.e. 2 x 3.7 = 7.4) to 4S (4 x 4.3 = 17.2V).

Once you have connected the battery, you should see the following:

- The green VBAT LED on;
- The orange 5V AUX LED on (if you have bridged the two left most 5V AUX pins), otherwise this LED will be off;
- You should be able to measure 5V on the test pins in the top left corner (again assuming AUX is bridged); and
- You should measure the battery voltage on the P1 and P2 test pins at the bottom centre of the board.

If you don’t see any of the above, immediately disconnect your battery and check for errors. If the above checks are ok you can disconnect your battery and then plug in the Nano 33 BLE. When you reconnect the battery your Nano should power up as normal. Make sure you plug the board in the right way round. The USB port should be towards the bottom of your board.