# otbr-snap
This is an attempt to snap Open Thread Border router

# Building 
- Snap currently only builds for arm64 platform. But building for other other architectures should no be big issue.
- Use following command build the snap . Please note that cross compilation is not tested.
  
  ```
   snapcraft --destructive-mode
  ```
# Running snap on arm64 board
- Need to have atleast one IPv4/IPv6 compatible network interface
  
  ```
 snap install avahi
 snap install bluez
 snap install otbr-snap_0.1_arm64.snap --dangerous
 
 ```

# bluez connections

```
 snap connect bluez:home
 snap connect bluez:network-control
 snap bluez:uinput
```
# otbr-snap connections 

```
snap connect otbr-snap:firewall-control
snap connect otbr-snap:network-control
snap connect otbr-snap:avahi-observe avahi:avahi-observe
snap connect otbr-snap:avahi-control avahi:avahi-control
snap connect otbr-snap:etc-iproute2-rttables
snap set otbr-snap infra-iface=<name of net interface>
```
# Testing
Testing needs installation of chip-tool snap
https://snapcraft.io/chip-tool
once chip-tool is installed and all connections are connected
This can be tested using NRF52840 RCP dongle and nRF52840 Lighting sample
Thread network formation can work as written down here
https://github.com/prash813/matter-otbr/blob/main/README.md

# Testing on RPi
testing on RPi might need pi-bluetooth snap.

# TODO-:
Rewriteing this guide.
