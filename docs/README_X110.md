# X110 sea floor deployment requirements

The seatrac X110 transceiver will be positioned near the sea floor.

## Requirements

1. The latitude and longitude of the device, in decimal degrees, are static and known
   to at least five decimal digits in WGS 84.
2. The depth of the device, in meters below MLLW, is static and known to within 50
   centimeters.
3. The altitude of the device, in meters above the sea floor, is static and known
   to within 50 centimeters.
4. The depth plus the altitude is less than 1,000 meters.
5. The device is oriented with the cable connector down and the transducer up.
6. The device is connected to an electrical power supply which provides
   regulated voltage between 12 Volts and 24 Volts and is capable of at least 500
   mAmps continuous.
7. The electrical power can be controlled remotely (turned OFF and ON).
8. The serial port on the device is connected to a Linux-based computer's RS-232C
   port. That computer can be accessed remotely.
9. The mounting plate of the device is on the great circle plane of a hemisphere. The
   plane of the hemisphere is parallel to the sea surface. The hemisphere has a
   radius of 1,000 meters. There is an unobstructed sonar path to any location inside
   the hemisphere.
10. The device is at least 1,000 meters from any other sonar device operating in the 
    24 kHz to 32 kHz band.
11. There exists a way to visually inspect the transducer for marine growth
    accumulation. There also exists a means to perform light cleaning of the
    transducer, in place on the sea floor.
