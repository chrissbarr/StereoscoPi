# StereoscoPi
StereoscoPi is a PCB that connects to a Raspberry Pi Compute Module. It powers the Compute Module, and provides connections for two cameras, along with a USB connection for a WiFi adaptor.

## Power
Power input is via a 2.1mm barrel jack. StereoscoPi uses a highly efficient switching regulator, and can accept 5V - 17V.

## Cameras
StereoscoPi is designed to work with the improved Raspberry Pi camera boards developed by [ArduCam](http://www.arducam.com/raspberry-pi-camera-rev-c-improves-optical-performance/). The connectors used are compatible with the standard Raspberry Pi camera boards, however the ArduCam boards are larger and feature different hole spacing.

## Connectivity
In order to minimise the size and weight of StereoscoPi, there is no onboard USB hub - this means only the single USB port integrated into the Broadcom SOC is available. This has been broken out to a full size USB-A socket.

Because the Raspberry Pi Compute module does not use removable storage as is the case with the other Raspberry Pi boards, it is necessary to connect the module to a computer and mount the flash memory as a mass storage device. This allows the copying over of all necessary operating system files. There is an onboard microUSB socket to facilitate this. As the Broadcom SOC only has the one USB connection, usually this connector is inactive. When the programming jumper is set appropriately, the full-size USB socket is disconnected, the microUSB socket is connected, and the board is set into programming mode.

Several additional outputs for power and PWM signals are broken out across the board. These are general purpose, but could - for example - be used to drive a small servo-based gimbal.

## Prototype
A proof-of-concept prototype has been constructed using two Raspberry Pi 2 B SBCs, and two separate wireless network adaptors. For information on replicating the hardware of this setup, follow the guide on the Prototype Hardware page. For software setup, see the Prototype Software page.


