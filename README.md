# Anchor Chain Contol with WLAN

This is a port from the ESP32 repository to a Wemos D1 Mini which is amaller than the ESP32 Node MCU.

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
- Current Chain Counter is stored in nonvolatile memory. ESP32 can be switched off after anchoring (counter is restored after new start).
- Demo mode to check functionality without having a windlass / chain counter connected to ESP32 (set ENABLE_DEMO to 1).

![Picture2](https://github.com/AK-Homberger/ESP8266_AnchorChainContol_WLAN/blob/main/SchematicsD1MiniChainCounterWLAN.png)

A pcb layout is available in the main folder: "D1MiniChainCounterWLAN.kicad_pcb".

You can order a PCB from Aisler.net: https://aisler.net/p/WJSHXVDM

![Board](https://github.com/AK-Homberger/ESP8266_AnchorChainContol_WLAN/blob/main/D1MiniChainCounterWLAN.png)


The current design should work for a Quick or Lofrans anchor chain relay and chain sensor (which looks like a simple reed relay triggerd from a magnet). Connection details  for a Quick windlass/counter can be found here: https://www.quickitaly.com/resources/downloads_qne-prod/1/CHC1203_IT-EN-FR_REV001A.pdf

The resistors R4, R5 and the transistors Q3, Q4 are currently not necessary. They shall support a manual override detection in the future (currently not yet imlemented in the code).

# Partlist:

- D1 Mini [Link](https://www.reichelt.de/de/en/d1-mini-esp8266-v3-0-d1-mini-p253978.html?&nbc=1)
- D24V10F5 [Link](https://eckstein-shop.de/Pololu-5V-1A-Step-Down-Spannungsregler-D24V10F5)
- Resistor 1 KOhm [Link](https://www.reichelt.de/de/en/carbon-film-resistor-1-4-w-5-1-0-kilo-ohms-1-4w-1-0k-p1315.html?&trstct=pos_2&nbc=1) Other resistors are the same type! Click on "5% Carbon film resistors" then two times "+ more filter" to select values.
- Transistor BC547 (* 2/4) [Link](https://www.reichelt.de/bipolartransistor-npn-45v-0-1a-0-5w-to-92-bc-547b-dio-p219082.html?search=BC547)
- Diode 1N4148 [Link](https://www.reichelt.de/schalt-diode-100-v-150-ma-do-35-1n-4148-p1730.html?search=1n4148)
- Diode 1N4001 [Link](https://www.reichelt.de/de/en/rectifier-diode-do41-50-v-1-a-1n-4001-p1723.html?&nbc=1)
- H11-L1 [Link](https://www.reichelt.de/optokoppler-1-mbit-s-dil-6-h11l1m-p219351.html?search=H11-l1)
- Relay (*2) [Link](https://www.reichelt.de/de/en/printrelais-2x-no-5-a-5-v-dc-g6b-2s-5vdc-p168487.html?CCOUNTRY=445&LANGUAGE=de&GROUPID=7621&START=0&OFFSET=16&SID=96Xk5YJngRlij1C8dm7WFa8cc43c9fd0145a715a7ea5bf81fdb75&LANGUAGE=EN&&r=1)
- Connector 2-pin (*3) [Link](https://www.reichelt.de/de/en/2-pin-terminal-strip-spacing-3-5-akl-059-02-p36598.html?&nbc=1)


# Updates:

02.11.2020 - Version 1.0: Initial version with Wemos D1 Mini.
