# triangl.io AP Setup
## 1. Install Image
1. Configure PC with static IP 192.168.0.66/24 (Subnetmask: 255.255.255.0) and start tftp server with `tftp 192.168.0.66`
2. Rename "openwrt-ramips-mt76x8-tplink_tl-wr902ac-v3-squashfs-tftp-recovery.bin"
   to "tp_recovery.bin" and place it in tftp server directory `/private/tftpboot` (mac).
3. Connect PC with the LAN port of the AP, press the reset button, power up
   the router and keep button pressed for around 6-7 seconds, until
   device starts downloading the file.
4. Router will download file from server, write it to flash and reboot. The install process take about 1 minute.
5. Connect `ssh root@192.168.1.1`, start with configuration
6. Optional: if message "WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!", do a `ssh-keygen -R 192.168.1.1`

## 2. Configure AP
### Activate DHCP in the network config.

1. Open `/etc/config/network`
2. Set `option proto 'static'` to ` option proto 'dhcp'` in the `lan` interface
3. Delete the following lines in the `lan` interface
4. Set a root password `passwd root`
5. Restart the AP `reboot` and connect it to the Factory Network (LAN)
6. Wait until our bot post the new ip address in the #hardware_update slack channel

Stores the configurations used to set up the mesh network using the openwrt operation system and the batman mesh algorithm.

### Change Welcome message
1. Connect `shh root@NEWIPADDRESS` with the new ip.
2. Go to `/etc` and open `banner`
3. Change to
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

### Fresh Installation notes

1. Clone repo locally, then transfer files to AP root directory. 
2. Connect `shh root@NEWIPADDRESS` to the router.
3. Execute the following commands:

```bash
opkg update
opkg install kmod-batman-adv
opkg install batctl
opkg remove wpad-mini
opkg install wpad-mesh
```

Move config files to `/etc/config/`

