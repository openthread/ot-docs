# Thread and Zigbee Sniffer

Follow these instructions to set up a sniffer to monitor a ZigBee/Thread network through a Silicon Labs evaluation board. Two different ways of achieving this will be discussed.

- EFR32 sniffer function
- Turning any EFR32 into a Zigbee or Thread sniffer

The sniffer configurator tool is ready to use in Simplicity Studio V4 (post Q4 2018 release) and V5 (all versions).

# EFR32 Sniffer Function

## Hardware Setup

Two sets of boards are used for this example:

- *2x* BRD4161A (radio board; any EFR32MG work for this     case)
- *2x* BRD4001A (development board)

![image-20210510135855643](C:\Users\yanecib\AppData\Roaming\Typora\typora-user-images\image-20210510135855643.png)

## Software

Load the Zigbee examples Z3Switch in one radio board and Z3Light in the other.

## Configure the Network Analyzer

Once your network is ready make sure the *Network Analyzer* is set to decode the correct protocol. Select *Window > Preferences > Network Analyzer >*

*Decoding > Stack Versions*, and verify it is set correctly. If you need to change it, click the correct stack then click *Apply* and *OK*.

![image-20210510135925207](C:\Users\yanecib\AppData\Roaming\Typora\typora-user-images\image-20210510135925207.png)

You will also need to get your network key (NWK key) to decrypt packets. Connect to one of your boards and launch the console. Under *Serial1* use the following command:

  **keys print**  

The console command should produce a display similar to this:

![image-20210510140002820](C:\Users\yanecib\AppData\Roaming\Typora\typora-user-images\image-20210510140002820.png)

In *Window > Preferences > Network Analyzer > Decoding > Security Keys*, click *New*, name the new entry and paste the copied key into it. Click *Apply and Close*.

![image-20210510140139133](C:\Users\yanecib\AppData\Roaming\Typora\typora-user-images\image-20210510140139133.png)

Now your analyzer is ready to display packets obtained from monitoring your network. 

Right-click on the light or the switch device, and select *Start Capture*. Do the same for the other device.

If you are in an environment with a large number of wireless devices, you may have a very noisy *Network Analyzer* environment, which will be reflected both in the event traffic and in the map. You can display additional information by clicking on the map.

To monitor with more features:

·    Click on one of the transactions.

- In the Event Detail, expand IEEE 802.15.4 and scroll     down until you see the Destination PAN ID.
- Right-click on it, and select *Add to filter,* hit     enter key.

·    By right-clicking on a device, you can also show connectivity and add labels (here we added Switch and Light).

![image-20210510140259924](C:\Users\yanecib\AppData\Roaming\Typora\typora-user-images\image-20210510140259924.png)

# Turning any EFR32 into a Zigbee or Thread Sniffer

## Hardware Setup

![image-20210510140325307](C:\Users\yanecib\AppData\Roaming\Typora\typora-user-images\image-20210510140325307.png)

We are going to use :

- *2x* EVB (EFR32 MG12) each one plugged over a BRD4001A
       One to create the NCP Thread
       The second one to create the sniffer

- *1x* Raspberry Pi 3B+

# Software

## Thread device

[Setting up a Thread NCP device using wpantund on raspberry pi]()

Once you have built your NCP Thread device you are ready to create your sniffer. 

```
$ sudo wpanctl
 
wpanctl:wpan0> leave
wpanctl:wpan0> setprop NetworkKey --data 00112233445566778899aabbccddeeff
wpanctl:wpan0> form "silabs" -c 26
wpanctl:wpan0> status
```

The status command should give you this result: 

**wpan0 => [**

```
"NCP:State" => "associated"
"Daemon:Enabled" => true
"NCP:Version" => "SL-OPENTHREAD/1.0.0.2 GitHub-f411a412bee; EFR32; Oct 6 2020 12:05:18"
"Daemon:Version" => "0.07.01 (0.07.01-2-g6993264; Jul 17 2020 21:32:08)"
"Config:NCP:DriverName" => "spinel"
"NCP:HardwareAddress" => [842E14FFFE906472]
"NCP:Channel" => 26
"Network:NodeType" => "leader"
"Network:Name" => "silabs"
"Network:XPANID" => 0xF45B7F4A9CD3AFA5
"Network:PANID" => 0x2EBA
"IPv6:LinkLocalAddress" => "fe80::24d7:395c:d7f4:8ad9"
"IPv6:MeshLocalAddress" => "fdf4:5b7f:4a9c:0:b12f:2650:befc:262d"
"IPv6:MeshLocalPrefix" => "fdf4:5b7f:4a9c::/64"
"com.nestlabs.internal:Network:AllowingJoin" => false
```

**]**

## Sniffer (for Thread or ZigBee)

Create a new *Railtest* sample application using the default profile and *PHY* settings (generate and compile using GCC or IAR).

Load the built binary image onto your chosen *EFR32* dev board.

Connect to the console of your *EFR32*. In order to turn the *Railtest* image into a sniffer for 802.15.4 protocols like ZigBee and Thread you will need to issue the following commands over the console:

```
This will place the radio into an idle state so that you can configure it.
 rx 0
 
This command configures the radio for the 802.15.4 PHY.
config2p4GHz802154
 
Sets the radio into the right mode for 802.15.4 packet capture.
enable802154 rx 100 192 864
 
Sets the radio into promiscuous capture mode so it can act as a sniffer.
setPromiscuousMode 1
 
Sets the desired channel you wish to sniff.
setChannel 26
 
Sets the radio back into receive mode.
rx 1
```


 Now you can capture packets from your device in the *Network Analyzer* and you should start seeing traffic (transactions/events) captured from the network.

For a Thread sniffer, check if the corresponding stack is selected : *Window > Preferences > Network Analyzer >Decoding > Stack Versions > OpenThread stack.* Select the correct stack and click *Apply* and *Close*.

For a ZigBee sniffer, return to *[Configure the Network Analyzer](https://confluence.silabs.com/pages/editpage.action?pageId=144554937#ThreadandZigbeesniffer-Configurethenetworkanalyzer)* under section *EFR32 sniffer function* above***.\***

# Results (Thread example)

![image-20210510140451442](C:\Users\yanecib\AppData\Roaming\Typora\typora-user-images\image-20210510140451442.png)