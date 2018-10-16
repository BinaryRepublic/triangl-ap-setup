# triangl.io AP Setup
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
1. Get kernel Version with `uname -r` (eg. 4.14.67)
2. Copy package folder based on the kernel version: `scp -r packages/4.14.67 root@192.168.1.1:/tmp/` (change version folder if necessary)
3. Go to `cd /tmp/4.14.67` 
4. Install packages:
```opkg install kmod-crypto-hash_4.14.67-1_mipsel_24kc.ipk
opkg install kmod-crypto-crc32c_4.14.67-1_mipsel_24kc.ipk
opkg install kmod-lib-crc32c_4.14.67-1_mipsel_24kc.ipk
opkg install kmod-lib-crc16_4.14.67-1_mipsel_24kc.ipk
opkg install kmod-batman-adv_4.14.67+2018.2-0_mipsel_24kc.ipk
```

### 3. Change welcome message
1. Go to `/etc` and open `vim banner`
2. Change to:
```
 ####### ########  ####    ###    ##    ##  ######   ##
   ##    ##     ##  ##    ## ##   ###   ## ##    ##  ##
   ##    ##     ##  ##   ##   ##  ####  ## ##        ##
   ##    ########   ##  ##     ## ## ## ## ##   #### ##
   ##    ##   ##    ##  ######### ##  #### ##    ##  ##
   ##    ##    ##   ##  ##     ## ##   ### ##    ##  ##
   ##    ##     ## #### ##     ## ##    ##  ######   ########

-------------------------------------------------------------
************* WIRELESS NODE AC:84:C6:E8:BE:79 ***************
-------------------------------------------------------------
```
3. Change mac address

### 4. Configure AP
1. Open `vim /etc/config/network` and copy the ula_prefix (config globals 'globals') to the local network file (folder: wireless_node), copy also the mac address (lan_dev) in the local network file.
2. Copy all files from wireless_node to `/etc/config`
3. Set root password `passwd root`
4. Restart

### 3. Configure AP

6. Wait until our bot post the new ip address in the #hardware_update slack channel

```bash
opkg update
opkg install batctl
opkg remove wpad-mini
opkg install wpad-mesh
```

