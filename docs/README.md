It's possible to run this code on host A and connect to a USBL
device connected to host B, provided there is a network path
between the two hosts.

```
KERNEL=="ttyUSB[0-9]*", ATTRS{serial}=="FT76BBVB", ATTRS{idVendor}=="0403", \
  ATTRS{idProduct}=="6001", SYMLINK+="seatrac/x150", MODE="0666"
KERNEL=="ttyUSB[0-9]*", ATTRS{serial}=="AU04GX3Z", ATTRS{idVendor}=="0403", \
  ATTRS{idProduct}=="6001", SYMLINK+="seatrac/x110", MODE="0666"
```

On host B run the following:
```
socat TCP6-LISTEN:55443,reuseaddr,fork \
      open:$USBL_PORT,nonblock,raw,echo=0,b115200,waitlock=/tmp/socat.lock &
```

On host A, run the following command:
```
socat pty,raw,nonblock,b115200,echo=0,link=/tmp/usbl0 \
      TCP6:$USBL_HOST:55443
```

# Node design

The design handles N kinds of datagrams coming from the device:

1. An intermittent stream of unsolicited datagrams. Two examples are Diagnostics and
   Errors.
2. A continuous stream of datagrams which are started and stopped by commands. One
   example is the Status message.
3. A single datagram sent in response to a single command. An example, and the raison
   d'être of the node, is the response to a Ping command.

## Configure the device

1. connect the serial port, separately for reading and writing
    pyserial allows one process to open the same serial device twice
1. retrieve the serial number
1. assign the ID number and configure
1. setup the callbacks for the inbound datagrams

## Datagram loop

Continuously loop through all of the following.

### Read incoming datagrams

A thread dedicated to continuously reading datagrams from the serial port, extracting
the CID, and executing an assigned callback. Most of the callbacks will extract
information from the datagram and use it to update one or more status objects. The
callback which handles the USBL information with range and bearing will publish a ROS
message.

### Write recurring datagrams

Loop through a list of recurring outbound datagrams and write them to the serial
port. Each datagram will include a transmission frequency. A typical example is the
PING command.

### Write queued datagrams

These are usually the startup configuration datagrams, for commands like INFO and SETTINGS.

## Notes about hardware

### X110

```
SysInfo
 Uptime seconds: 1041
 Application: Main
 Hardware: {'part_number': 'X150', 'part_rev': 6, 'serial_number': 28253, 'flags_sys': 0, 'flags_user': 0}
 Boot firmware: {'valid': True, 'part_number': 912, 'version': '1.6.436', 'cksum': 0}
 Main firmware: {'valid': True, 'part_number': 913, 'version': '2.4.2206', 'cksum': 3108036330}
 Board revision: 112
Device ID: 1
Settings already up-to-date
```

### X150

```
SysInfo
 Uptime seconds: 8
 Application: Main
 Hardware: {'part_number': 'X150', 'part_rev': 6, 'serial_number': 28244, 'flags_sys': 0, 'flags_user': 0}
 Boot firmware: {'valid': True, 'part_number': 912, 'version': '1.6.436', 'cksum': 0}
 Main firmware: {'valid': True, 'part_number': 913, 'version': '2.4.2206', 'cksum': 3108036330}
 Board revision: 16
Device ID: 15
Settings already up-to-date
```
