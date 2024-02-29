# Make a Raspberry Pi a wifi access point that forward traffic to a VPN

UPDATE: from time to time dns are shut down. Lookup a reliable DNS from https://dnschecker.org/public-dns/cn
and input its ip address into /etc/dhcpcd.conf
----


We will configure these devices:
- wlan0: internal wifi that is used to create the access point
- wlan1: a USB Wifi dongle used to connect to internet (house wifi router)
- tun0: openvpn tunnel to remote country

Then we will forward traffic as indicated:
 
    Device -> wlan0 -> tun0 -> (wlan1 -> router -> ISP ->)  VPN service

We will use NAT from wlan0 to tun0 (not a bridge)

## wlan1: connection to home network
View if wifi usb dongle is correcly installed
    
    dmesg
    lsusb
    iwconfig

 The device wlan1 shoud appear, otherwise install usb dongle firmware 
 
     sudo apt-get install firmware-realtek
     sudo apt-get install zd1211-firmware  # download directly from http://mirrors.pidginhost.com/raspbian/raspbian/pool/non-free/z/zd1211-firmware/firmware-zd1211_1.5-6_all.deb

Otherwise https://wiki.debian.org/zd1211rw

If still dows not work, for example we have a Cudy WU700 with RTL8811CU, we need to recompile the firmware. Follow instructions from https://github.com/fastoe/RTL8811CU_for_Raspbian

    git clone -b v5.8.1 https://github.com/fastoe/RTL8811CU_for_Raspbian
    sudo apt install -y bc git dkms build-essential raspberrypi-kernel-headers
    cd RTL8811CU_for_Raspbian
    make
    sudo make install
    sudo modprobe 8821cu
    sudo reboot

wlan1 should get the wifi configuration from wpa_supplicant

    sudo nano /etc/dhcpcd.conf

Put there lines so that in case of failure from router, it gets an ip in any case:
    
    interface wlan1
      fallback static

## wlan0: access point
As it is the internal wifi, it should not need additional firmware. We need to disable its use of wpa_supplicant, so that it doesn't connect to home network

### Static IP
wlan0 should get a fixed ip address, as it is the access point gateway. Ip addresse are managed by dhcpcd.conf (do not use the old /etc/network/interfaces).

    sudo nano /etc/dhcpcd.conf

Put these at the end:

    interface wlan0
      nohook wpa_supplicant
      static ip_address=192.168.4.1/24
      static domain_name_servers=223.6.6.41
      #other China DNS options from https://public-dns.info/nameserver/cn.html
      #223.5.5.5 223.5.5.0 dna alibaba hangzhou
      #103.144.53.238 guangdong

    
The static domain_name_servers can be read and put the same as tun0.openvpn, so that DNS requests are forwarded only to the VPN

    resolvconf -l


### Access point
Install and configure hostapd to create the access point on wlan0

    sudo apt install hostapd
    sudo nano /etc/hostapd/hostapd.conf

For WPA2 authentication and 2.4 Ghz (channel 1), put this text:

    interface=wlan0
    driver=nl80211  
    hw_mode=g
    channel=1
    wpa=2
    wpa_key_mgmt=WPA-PSK
    rsn_pairwise=CCMP
    wpa_pairwise=TKIP
    
    ssid=china
    wpa_passphrase=my_password



### DHCP
wlan0 will give ip addresses to connected devices, so we need a DHCP server

    sudo apt install hostapd dnsmasq
    sudo mv /etc/dnsmasq.conf /etc/dnsmasq.conf.orig
    sudo nano /etc/dnsmasq.conf

Put these lines at the end:

    interface=wlan0
    dhcp-range=192.168.4.2,192.168.4.20,255.255.255.0,24h


## tun0: vpn to CyberGhost
Create the zip from https://my.cyberghostvpn.com/ following manual setup.
Download the configuration, and install it like https://support.cyberghostvpn.com/hc/en-us/articles/213813045-Raspberry-Pi-How-to-configure-a-Raspberry-Pi-as-a-web-proxy-with-OpenVPN but without privoxy

    sudo mv raspberry_openvpn.zip /etc/openvpn
    cd /etc/openvpn
    unzip raspberry_openvpn.zip
    sudo cp openvpn.ovpn CG_CH.conf
    sudo nano CG_CH.conf

Extend the line ‘auth-user-pass’

    auth-user-pass /etc/openvpn/user.txt

At the bottom of the configuration passage (after 'verb 4') add the following two lines:

    up /etc/openvpn/update-resolv-conf
    down /etc/openvpn/update-resolv-conf

Add username an password to user.txt

    sudo nano user.txt
    sudo nano /etc/default/openvpn
    
AUTOSTART="CG_XX"  (the name of the file WITHOUT the file extension ‘.conf’)

    sudo update-rc.d openvpn enable
    sudo service openvpn start

### DEBUGGING
Openvpn in  installed in 
 
    sudo nano /etc/init.d/openvpn
 
To view if vpn is running (even if it is configured using sysv update-rc.d and init.d)

    systemctl list-units | grep vpn

To configure services on sysv

    sudo sysv-rc-conf
 

## Traffic forwarding
Enable ip forwarding

    sudo nano /etc/sysctl.conf

Put "1" on the line:

    net.ipv4.ip_forward=1

DEBUG: Reset iptables

    sudo iptables -F
    sudo iptables -X
    sudo iptables -Z
    sudo iptables -t nat -F
    sudo iptables -t nat -X
    sudo iptables -t nat -Z
    sudo iptables -t mangle -F
    sudo iptables -t mangle -X
    sudo iptables -t mangle -Z
    sudo iptables -P INPUT ACCEPT
    sudo iptables -P OUTPUT ACCEPT
    sudo iptables -P FORWARD ACCEPT

## Traffic forwarding from wlan0 to tun0

    sudo iptables -t nat -A POSTROUTING -o tun0 -j MASQUERADE
    sudo iptables -A FORWARD -i tun0 -o wlan0 -m state --state RELATED,ESTABLISHED -j ACCEPT
    sudo iptables -A FORWARD -i wlan0 -o tun0 -j ACCEPT
    sudo apt-get install iptables-persistent
 
To forward DNS requests from wlan0 to tun0, so that DNS spoofing cannot be detected using https://whoer.net or https://dnsleaktest.com/
Read the DNS for tun0, and put the same in wlan0
    
    resolvconf -l
    sudo nano /etc/dhcpcd.conf

Update static domain_name_servers

    interface wlan0
      nohook wpa_supplicant
      static ip_address=192.168.4.1/24
      static domain_name_servers=10.0.0.243


Verify that Youtue (blocked) give different DNS results:

    nslookup youtube.com
    nslookup youtube.com 10.0.0.243

