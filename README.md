# ESP12 PCB for ESPAltherma

## Power
The normal use case, i.e. operating in the HP, is for the HP to power the board. JP1 is on and NO OTHER POWER is supplied via the USB connector, adjacent terminal block, or via the Serial header.

For flashing via the Serial header, generally have no other connections and power the board from the connected USB/Serial adapter, which should supply 3.3V (and have sufficient current capacity for the ESP9266), and connect JP2. In this set-up, the relays will not operate, as they are on the 5V rail.

Alternatively, disconnect SJ2 and power with 5V via the USB socket or terminal block. In this set-up, the relays will operate.

It is probably bad practice to connect anything to the Serial header when the board is connected to the HP, except for a BT module (see below).

## Runtime Switches
__S1__ (Reset) - ESP-12 reset.
__S2__ is optional, see notes on Serial, below

## Headers
### Serial
UART pins and power, intended for flashing the device and diagnostic monitoring, although some applications might use them. When using a Bluetooth serial monitor, JP6 should be on, to supply it with power. Note that the pin labels are to match those on the serial device, rather than being those of the ESP8622.  See note on JP2. 3.3V logic must be used.

Note that, if the DTR and RTS signals are connected, then the PlatformIO monitor (or any other serial interaction which is not bootloading) must be configured as follows (add to platformio.ini), otherwise attempts to use the Monitor cause the board to hang (recovers if the monitor is killed):
```
monitor_rts = 0
monitor_dtr = 0
```

If DTR is not available on the USB/Serial adapter, install S2 and R19. Cut through SJ2 and SJ3 for manual boot-loader trigger.
__S2__ (Flash) -  Reset will trigger the boot loader if this switch is held on (i.e. flashing is done by holding S2 and then pressing/releasing S1). Once the new programme has been flashed, S1 (reset) must be done manually.

## Options
### LED Signalling
The battery of LEDs 1-7 and associated R12-18, plus JP3 allow for the output states to be signalled. JP3 allows for them to be disconnected/reconnected as required.

If the LEDs are not fitted then the pad nearest to "L1", "SG1", etc may be used as a test point.

Note that several of the Power Limitation outputs are active during ESP8266 boot; flashes will be seen on L1-L3.

### Bluetooth Monitoring
May be achieved by connecting a BT Serial module to the Serial header. Connect JP2 to power the module. NB this is 3.3V.

### Power Supply
The USB connector and nearby terminal connector are for optional external power supply of 5V. JP1 must be disconnected.

### Omittable Components
The ESP-12 modules have a LED on GPIO2, which may be unwanted (this is L2)!
