# How to properly install No-IP Dynamic Update Client (DUC) under Raspbian

Compile the DUC Client

```
cd /usr/local/src
sudo wget http://www.no-ip.com/client/linux/noip-duc-linux.tar.gz
sudo tar xzf noip-duc-linux.tar.gz
cd noip-2.1.9-1
sudo make
sudo make install
```

Setting up the No-IP DUC Client service

```
cd /usr/local/src
sudo wget http://download.sizious.com/rpi/noip-duc-raspbian.tar.bz2
sudo tar -xjvf noip-duc-raspbian.tar.bz2
cd noip-2.1.9-1
sudo chmod +x raspbian.noip2.sh service.install.sh service.uninstall.sh
sudo ./service.install.sh raspbian
```

Checking if the No-IP service is working

```
systemd-analyze blame
```

For checking if the No-IP DUC Client Service is running, just enter the following command:

```
systemctl status noip2
```

To get more information on the noip2 instance running, enter the following command:

```
sudo noip2 -S
```

To start / stop service

```
sudo service noip2 start
sudo service noip2 stop
sudo service noip2 restart

# alternative command
sudo systemctl stop noip2

```

Check for error messages thrown at startup by running the following command

```
dmesg | grep "noip2"
```

## Auto renew

[github](https://github.com/loblab/noip-renew)

```
git clone https://github.com/loblab/noip-renew.git
```

Run `setup.sh` and set your noip.com account information

Run `noip-renew.sh`, check results.png (if succeeded) or error.png (if failed)

```
grep -h Confirmed *.log | grep -v ": 0" | sort
```