# How to Install the No-IP DUC on Raspberry Pi

You will be able to install No-IP’s Dynamic Update Client on Raspberry Pi in just a few minutes using Terminal. Installing the service is simple to do, and requires little knowledge of Linux.

You will need to create a directory for the client software to be installed.

Open Terminal and type the following. After each entry press “Enter“.   

```
mkdir /home/pi/noip
cd /home/pi/noip
```

After creating the folders for the DUC it is time to download the software.

Within the Terminal window  type the following. After each entry you will press “Enter”.

```
wget https://www.noip.com/client/linux/noip-duc-linux.tar.gz

tar vzxf noip-duc-linux.tar.gz
```

Next navigate to the directory you created to locate the downloaded files.

```
cd noip-2.1.9-1
```

Now install the program.

```
sudo make
sudo make install
```

After typing “sudo make install” you will be prompted to login with your No-IP account username and password.

After logging into the DUC answer the questions to proceed. When asked how often you want the update to happen you must choose 5 or more. The interval is listed in minutes, if you choose 5 the update interval will be 5 minutes. If you choose 30 the interval will be 30 minutes.

```
sudo /usr/local/bin/noip2
```

To confirm that the service is working properly you can run the following command.

```
sudo noip2 ­-S (Capital “S”)
```

## Auto Start on Reboot

edit `rc.local`

```
cd /etc
sudo nano rc.local
```

add this text to `rc.local`

```text
echo "Starting noip2."
/usr/local/bin/noip2
```