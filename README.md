# otbr-snap
This is an attempt to snap Open Thread Border router

# Building 
- Snap currently only builds for arm64 platform. But building for other other architectures should not be big issue.
- Use following command to build the snap . Please note that cross compilation is not tested.
  
  ```
   snapcraft --destructive-mode
  ```
# Running snap on arm64 board
- Need to have atleast one network interface. Wifi or Ethernet 
  
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
# otbr-snap connections and configs

```
	snap connect otbr-snap:firewall-control
	snap connect otbr-snap:network-control
	snap connect otbr-snap:avahi-control avahi:avahi-control
	snap connect otbr-snap:etc-iproute2-rttables
	snap connect otbr-snap:raw-usb
	snap set otbr-snap infra-iface=<name of net interface>

```



# Testing
Testing needs installation of chip-tool snap
 [Matter Controller snap](https://snapcraft.io/chip-tool)
 
 - Install chip-tool snap.
 	
	```
	snap install chip-tool
	
	```
 - Connect following interface

```
 snap connect chip-tool:bluez bluez:service

```

## Testing with  NRF52840 RCP dongle and nRF52840 Lighting sample
 - Please note that for testing with nRF dongles they need to be programmed.
   One dongle need to be programmed with RCP(Radio Co-Processor) firmware.
   Second dongle need to be programmed with Matter Lighting sample.
   This programming procedure is explained in the following Repo.
   [Programming nRF Dongles](https://github.com/prash813/matter-otbr.git)
  
   This link also explains how to work with these dongles in terms putting them into different opeartion modes.
   One important mode is,  putting Lighting Sample Dongle into commissioning mode, which is done by pressing
   push button(SW0) for 5 to 6 seconds.

 - Ensure that nRF RCP dongle is connected to board which will act as matter-controller. Like RPi or Mediatek board with Ubuntu Core 22
   Other can be connected anywhere, in a sense that it remains reachable over Bluetooth/Thread. 
 
 - Thread network need to be configured which can be done by 
   ot-ctl application in otbr-snap.

```
	otbr-snap.ot-ctl dataset init new
	otbr-snap.ot-ctl dataset commit active
	otbr-snap.ot-ctl ifconfig up
	otbr-snap.ot-ctl thread start
	sleep 5
	otbr-snap.ot-ctl state
	otbr-snap.ot-ctl netdata show
	otbr-snap.ot-ctl ipaddr
	otbr-snap.ot-ctl dataset active -x

```
 - The last command will print something like following which is called as thread network creds.
 ```
0e080000000000010000000300000e35060004001fffe002086221e3c5f32a45e50708fd0c3dd49e5bf56a051086d65296308e693127fec091b7196110030f4f70656e5468726561642d33376235010237b50410d1aadf84aeff15923b554f359431accc0c0402a0f7f8
 
 ```

 - Once thread network is formed chip-tool can be used to pair the lighting app nRF dongle 
 
 ```
	chip-tool pairing ble-thread <nodeid> hex:<thread NW creds> 20202021 3840

 ```
 - Once Pairing is successful. You can control thread lighting device using thread RCP
 
 ```
	chip-tool onoff toggle  <nodeid> <endpoint> 

 ```

# Testing on RPi
- Testing on RPi might need pi-bluetooth snap.
  [pi-bluetooth-22 snap](https://github.com/prash813/pi-bluetooth.git)

- Need to program the bluetooth module using ``` pi-bluetooth.btuart ```
- ``` bluetoothctl >> power on ```
- ensure that hciconfig shows following

```
hci0:   Type: Primary  Bus: UART
        BD Address: E4:5F:01:EE:35:21  ACL MTU: 1021:8  SCO MTU: 64:1
        UP RUNNING 
        RX bytes:1478 acl:0 sco:0 events:84 errors:0
        TX bytes:1234 acl:0 sco:0 commands:84 errors:0

```
- This should be enough for Matter controller to work with pi ble module.

# TODO-:
Rewriting this guide.
