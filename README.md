# ESP12 PCB for ESPAltherma

## Power
The normal use case, i.e. operating in the HP, is for the HP to power the board. JP1 is effectively an emergency disconnect feature and is normally on.

## Flashing
The firmware supports "Over The Air" updating but initial flashing will be via the Serial header. This initial flashing should be done without X10A being connected to the heat pump.

If testing with a 5V USB/Serial adapter attached to X10A then JP2 should be removed.

## Runtime Switches
__S1__ (Reset) - ESP-12 reset.
__S2__ is optional, see notes on Serial, below

## Headers
### Serial
UART pins and power, intended for initial flashing the device and diagnostic monitoring using a BT module. When using a Bluetooth serial monitor, JP2 should be on, to supply it with power. Note that the pin labels are to match those on the serial device, rather than being those of the ESP8622.  3.3V logic must be used.

Note that, if the DTR and RTS signals are connected (and D2 installed), then the PlatformIO monitor (or any other serial interaction which is not bootloading) must be configured as follows (add to platformio.ini), otherwise attempts to use the Monitor cause the board to hang (recovers if the monitor is killed):
```
monitor_rts = 0
monitor_dtr = 0
```

If DTR is not available on the USB/Serial adapter, install S2 and R16 and cut through SJ1 and SJ2 for manual boot-loader trigger.
__S2__ (Flash) -  Reset will trigger the boot loader if this switch is held on (i.e. flashing is done by holding S2 and then pressing/releasing S1). Once the new programme has been flashed, S1 (reset) must be done manually.

## Off-board "Thermo" Relay
See the ESPAltherma documentation for notes on the use of a relay to simulate a thermostat in order to on/off control the HP. This is a mains voltage relay AFAIK and in the interest of safety I decided to keep mains off the PCB. Hence there is a header to connect the GPIO to an off-board relay (which should ideally have an opto-coupler and suitable isolation gaps). A solder-jumper (SJ3) is used to allow selection of 3.3V or 5V for the header.

## Options
### LED Signalling
The battery of LEDs and associated Rs, plus JP3 allow for the output states to be signalled. JP3 allows for this signalling to be disconnected/reconnected as required. The external "thermo" relay is assumed to have its own signal.

If the LEDs are not fitted then the pad nearest to "L1", "SG1", etc may be used as a test point.

The ESP-12 modules have a LED on GPIO2, which is used as a "booted and running OK" signal.

### Bluetooth Monitoring
May be achieved by connecting a BT Serial module to the Serial header. Connect JP2 to power the module. NB this is 3.3V.

### Smart Grid OR Power Limitation
These two features use the same GPIO; only fit one set of components!

