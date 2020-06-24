# Pi VPN

Type of VPN
* WireGuard : new light-weight and fast
* OpenVPN : More common VPN

```

$ curl -L https://install.pivpn.io | bash

 WireGuard default port 51820
 OpenVPN default port 1094

$ pivpn add

 config file will be staore at '/home/pi/configs'

$ pivpn help

```

## Reset Tvheadend
 
1. Delete `hts/.hts/tvheadend`
   + `sudo rm -rf ./tvheadend`
2. `sudo service tvheadend stop`
3. `sudo dpkg-reconfigure tvheadend`
4. `sudo service tvheadend start`