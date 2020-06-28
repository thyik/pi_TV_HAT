# Test Raspberry Network Speed

## iperf

```
$ sudo apt-get install iperf

# start server
$ iperf -s

```

## ethtool

```
$ sudo apt-get install ethtool

# set link speed to 1000Mbps
$ sudo ethtool -s eth1 speed 1000 duplex full autoneg on

# display LAN setting
$ ethtool eth1

```

## Test result

*** Raspberry Pi B+ ***
* Result gigabit lan : 80Mbps
* Built-in LAN : 50Mbps

*** Raspberry Pi 3 ***
* 90Mbps
