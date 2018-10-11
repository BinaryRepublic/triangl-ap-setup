# triangl.io AP Setup
## 1. Install Image
1. Configure PC with static IP 192.168.0.66/24 (Subnetmas: 255.255.255.0) and start tftp server with `tftp 192.168.0.66`
2. Rename "openwrt-ramips-mt76x8-tplink_tl-wr902ac-v3-squashfs-tftp-recovery.bin"
   to "tp_recovery.bin" and place it in tftp server directory.
3. Connect PC with the LAN port of the AP, press the reset button, power up
   the router and keep button pressed for around 6-7 seconds, until
   device starts downloading the file.
4. Router will download file from server, write it to flash and reboot.
5. Connect `ssh root@192.168.1.1`, start with configuration
6. Optional: if message "WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!", do a `ssh-keygen -R 192.168.1.1`

## 2. Configure AP
### Activate DHCP in the network config.

1. Open `/etc/config/network`
2. Set `option proto 'static'` to ` option proto 'dhcp'` in the `lan` interface
3. Delete the following lines in the `lan` interface
4. Set a root password `passwd root`
5. Restart the AP `reboot` and connect it to the Factory Network (LAN)

Stores the configurations used to set up the mesh network using the openwrt operation system and the batman mesh algorithm.


### Fresh Installation notes

Clone repo locally, then transfer files to AP root directory. 

Ssh to ap, then execute:

```bash
opkg update
opkg install kmod-batman-adv
opkg install batctl
opkg remove wpad-mini
opkg install wpad-mesh
```

Move config files to `/etc/config/`

