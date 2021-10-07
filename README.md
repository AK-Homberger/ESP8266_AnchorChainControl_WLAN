# Anchor Chain Control with WLAN

This is a port from the ESP32 repository to a ESP8266 D1 Mini which is smaller than the ESP32 Node MCU.

## Caution: This is the updated version 1.2 with changed I/O pin definitions! The PCB is also updated but untested.

This repository shows how to remotely control the anchor chain relay via WLAN from phone/tablet.
Anchor chain events from the chain sensor are measured and presented on the phone/tablet.

Just set the WLAN ssid and password according to your needs. 
Select WLAN type with setting WiFiMode_AP_STA to "0" means Acess Point, or "1" means Client with DHCP.

Also set Chain_Calibration_Value according to your sensor (e.g. 0.33. meter per event).

If working as Access Point, connect the phone/tablet to the defined AP and start "192.168.4.1" in the browser.
If working as WLAN client, check the DHCP IP address with Serial Monitor of IDE and start browser with the shown IP address.

![Picture1](https://github.com/AK-Homberger/ESP32_ChainCounter_WLAN/blob/master/IMG_1254.PNG)

To control the anchor chain relay just press:
- "Down" for anchor down
- "Up" for anchor up
- "Stop" for Stop
- "Reset" to reset the chain counter to zero

Features:
- Saftey stop to stop "anchor up" two events before reaching zero (can be changed in code with SAFETY_STOP).
- Safety stop if maximum chain lenght is reached (standard 40 meters, can be changed with MAX_CHAIN_LENGTH)
- Watchdog timer to stop power after 1 second inactivity of client (e.g. due to connection problems).
- Watchdog timer to detect blocking chain. Engine stops if no events are detected within 1 second for up/down command.
- Current Chain Counter is stored in nonvolatile memory. ESP can be switched off after anchoring (counter is restored after new start).
- Demo mode to check functionality without having a windlass / chain counter connected to ESP (set ENABLE_DEMO to 1).

![Picture2](https://github.com/AK-Homberger/ESP8266_AnchorChainContol_WLAN/blob/main/SchematicsD1MiniChainCounterWLAN.png)

A [pcb layout](https://github.com/AK-Homberger/ESP8266_AnchorChainContol_WLAN/blob/main/D1MiniChainCounterWLAN-Board.pdf) is available in the [KiCAD](https://github.com/AK-Homberger/ESP8266_AnchorChainControl_WLAN/tree/main/D1MiniChainCounterWLAN) folder: "D1MiniChainCounterWLAN.kicad_pcb".

You can order a PCB from Aisler.net: https://aisler.net/p/WJSHXVDM

![Board](https://github.com/AK-Homberger/ESP8266_AnchorChainContol_WLAN/blob/main/D1MiniChainCounterWLAN.png)

The current design should work for a Quick or Lofrans anchor chain relay and chain sensor (which looks like a simple reed relay triggerd from a magnet). Connection details  for a Quick windlass/counter can be found here: https://www.quickitaly.com/resources/downloads_qne-prod/1/CHC1203_IT-EN-FR_REV001A.pdf

Used I/O pins:

- D1/D2: Output, relais Up/Down
- D6/D7: Input, manual override detection Up/Down (option)
- D5: Input, chain sensor 

With these IO pins the boot problem with the relais should be solved. Details see here: https://randomnerdtutorials.com/esp8266-pinout-reference-gpios/
The former version used D4/D5 as relais output istead of safe GPIO4(D1) and GPIO5(D2). D5,D6,D7 are safe for input.

The manual override detection function with D6/D7 is currently missing in the code. Feel free to support/update.

If you have problems with the chanin counter input measuring multiple count signals, it might be necessary to use a library to avoid multiple counts e.g.: https://github.com/mathertel/OneButton

You can easily do your own changes now: I added the full KiCAD details: https://github.com/AK-Homberger/ESP8266_AnchorChainControl_WLAN/tree/main/D1MiniChainCounterWLAN

## Parts:

- Board [Link](https://aisler.net/p/WJSHXVDM)
- U1 D1 Mini [Link](https://www.reichelt.de/de/en/d1-mini-esp8266-v3-0-d1-mini-p253978.html?&nbc=1)
- J2 D24V10F5 [Link](https://eckstein-shop.de/Pololu-5V-1A-Step-Down-Spannungsregler-D24V10F5)
- R1 Resistor 1 KOhm [Link](https://www.reichelt.de/de/en/carbon-film-resistor-1-4-w-5-1-0-kilo-ohms-1-4w-1-0k-p1315.html?&trstct=pos_2&nbc=1)
- R2, R3 Resistor 270 Ohm (*2) [Link](https://www.reichelt.de/de/en/carbon-film-resistor-1-4-w-5-270-ohm-1-4w-270-p1390.html?&nbc=1)
- R4, R5 Resistor 10 KOhm (*2) [Link](https://www.reichelt.de/de/en/carbon-film-resistor-1-4w-5-10-kilo-ohms-1-4w-10k-p1338.html?&nbc=1)
- Q1, Q2, Q3, Q4 Transistor BC337 (*4) [Link](https://www.reichelt.de/de/en/transistor-to-92-bl-npn-45v-800ma-bc-337-25-dio-p219125.html?&nbc=1)
- D2, D3 Diode 1N4148 (*2) [Link](https://www.reichelt.de/schalt-diode-100-v-150-ma-do-35-1n-4148-p1730.html?search=1n4148)
- D1 Diode 1N4001 [Link](https://www.reichelt.de/de/en/rectifier-diode-do41-50-v-1-a-1n-4001-p1723.html?&nbc=1)
- U2 H11-L1 [Link](https://www.reichelt.de/optokoppler-1-mbit-s-dil-6-h11l1m-p219351.html?search=H11-l1)
- K1, K2 Relay (*2) [Link](https://www.reichelt.de/de/en/miniature-power-relay-g5q-1-no-5-v-dc-5-a-g5q-1a-eu-5dc-p258331.html?&nbc=1)
- J1, J3, J4 Connector 2-pin (*3) [Link](https://www.reichelt.de/de/en/2-pin-terminal-strip-spacing-5-08-akl-101-02-p36605.html?&nbc=1)


## Updates:

- 06.10.2021 - Version 1.2: Changed IO pins to avoid problem during boot and preparation for correct manual override detection.
- 06.11.2020 - Changed PCB layout to version 1.1. Larger terminal blocks (5.08 mm) and different transistor footprints for easy soldering.
- 02.11.2020 - Version 1.0: Initial version with ESP8266 D1 Mini.
