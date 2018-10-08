# router-configuration
Stores the configurations used to set up the mesh network using the openwrt operation system and the batman mesh algorithm.


## Fresh Installation notes

Clone repo locally, then transfer files to AP root directory. 

Ssh to router, then execute:

```bash
opkg update
opkg install kmod-batman-adv
opkg install batctl
opkg remove wpad-mini
opkg install wpad-mesh
```

Move config files to `/etc/config/`

