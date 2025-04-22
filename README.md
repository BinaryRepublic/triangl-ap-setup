# tirangl

Triangl was a [CODE](https://code.berlin) project in WS-2018 designed to anonymously track smartphones and smart devices inside shopping malls, stores, or airports. The system works without requiring devices to connect to WiFi - it only needs their WiFi to be turned on, which is the default for most phones and watches.
This works because smartphones and smartwatches regularly broadcast "probe requests" searching for WiFi networks, even when not connected. We placed multiple WiFi access points throughout a space, connected in a mesh network running a custom kernel. These access points passively listen for these probe signals from devices and send that information to our back-end.

When three or more access points detect a signal from the same device, our system calculates its position using signal strength and time-of-flight lateration. The tracking data goes through a processing pipeline that converts it to an easily queryable format and stores it in an analyzing database.
Businesses can use this tracking data to analyze customer flow patterns, generate heatmaps of high-traffic areas, and accurately count customers throughout their space. Our customers can access all this analyzed data through a dashboard to understand movement patterns and optimize their layouts.

**Architectural overview:** https://github.com/binaryrepublic/triangl-infrastructure#overview

**Tracking-Ingestion Service:** https://github.com/BinaryRepublic/triangl-tracking-ingestion-service

**Processing Pipeline:** https://github.com/BinaryRepublic/triangl-processing-pipeline

## triangl AP Setup

Here is a introduction guide to flash OpenWRT on a TP-Link TL-WR902AC (V3) and install the triangl software applications.

Download the TFP Recovery Image from https://openwrt.org/toh/hwdata/tp-link/tp-link_tl-wr902ac_v3

### 1. Install Image
1. Configure PC with static IP 192.168.0.66/24 (Subnetmask: 255.255.255.0) and start tftp server with `tftp 192.168.0.66`
2. Rename "openwrt-ramips-mt76x8-tplink_tl-wr902ac-v3-squashfs-tftp-recovery.bin"
   to "tp_recovery.bin" and place it in tftp server directory `/private/tftpboot` (mac).
3. Connect PC with the LAN port of the AP, press the reset button, power up
   the router and keep button pressed for around 6-7 seconds, until
   device starts downloading the file.
4. Router will download file from server, write it to flash and reboot. The install process take about 1 minute.
5. Connect `ssh root@192.168.1.1`, start with configuration
6. Optional: if message "WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!", do a `ssh-keygen -R 192.168.1.1`

### 2. Install Packages
1. Get kernel Version with `uname -r` (eg. 4.14.68)
2. Copy package folder based on the kernel version: `scp -r packages/4.14.68 root@192.168.1.1:/tmp/` (change version folder if necessary)
3. Go to `cd /tmp/4.14.68` 
4. Install packages:
```
opkg install kmod-crypto-hash_4.14.67-1_mipsel_24kc.ipk
opkg install kmod-crypto-crc32c_4.14.67-1_mipsel_24kc.ipk
opkg install kmod-lib-crc32c_4.14.67-1_mipsel_24kc.ipk
opkg install kmod-lib-crc16_4.14.67-1_mipsel_24kc.ipk
opkg install kmod-batman-adv_4.14.67+2018.2-0_mipsel_24kc.ipk
```

### 3. Change welcome message
1. Go to `/etc` and open `vim banner`
2. Change to:
```

                   88                                    88
  ,d               ""                                    88
  88                                                     88
MM88MMM 8b,dPPYba, 88 ,adPPYYba, 8b,dPPYba,   ,adPPYb,d8 88
  88    88P'   "Y8 88 ""     `Y8 88P'   `"8a a8"    `Y88 88
  88    88         88 ,adPPPPP88 88       88 8b       88 88
  88,   88         88 88,    ,88 88       88 "8a,   ,d88 88
  "Y888 88         88 `"8bbdP"Y8 88       88  `"YbbdP"Y8 88
                                              aa,    ,88
                                               "Y8bbdP"

-------------------------------------------------------------
************** GATEWAY NODE AC:84:C6:E8:C9:D4 ***************
-------------------------------------------------------------
```
3. Change mac address

### 4. Configure AP
1. Open `vim /etc/config/network` and copy the ula_prefix (config globals 'globals') to the local network file (folder: wireless_node), copy also the mac address (lan_dev) in the local network file.
2. Copy all files from wireless_node to `/etc/config`
3. Set root password `passwd root`
4. Restart `reboot`

### 5. Install additional packages

1. Wait until our bot post the new ip address in the #hardware_update slack channel.
2. Connect via ssh to the ap.
3. Install packages

```bash
opkg update
opkg install batctl
opkg remove wpad-mini
opkg install wpad-mesh
opkg install curl
opkg install jq
```

