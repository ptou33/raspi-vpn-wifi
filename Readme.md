Make a Raspberry Pi a wifi access point that forward traffic to a VPN

Devices:
- wlan0: internal wifi that is used to create the access point
- wlan1: a USB Wifi dongle used to connect to internet (house wifi router)
- tun0: openvpn tunnel to remote country

Device -> wlan0 -> tun0 -> (wlan1 -> router -> ISP ->)  VPN service

First configure wlan1 to access router wifi



Configure tun0
Create the zip from https://my.cyberghostvpn.com/ following manual setup.
Download the configuration

unzip raspberry_openvpn.zip
mkdir cyberghost
mv ca.crt cyberghost/
mv client.crt cyberghost/
mv client.key cyberghost/
mv openvpn.ovpn cyberghost/

sudo cp * /etc/openvpn/

cd /etc/openvpn/
sudo nano user.txt

sudo nano CG_CH.conf

 1552  up /etc/openvpn/update-resolv-conf
 1553  sudo nano /etc/default/openvpn
 1554  sudo update-rc.d openvpn enable
 1555  sudo service openvpn start
 1556  sudo reboot
 1557  unzip raspberry_tcp_openvpn.zip cyberghost_tcp
 1558  mkdir cyberghost_tcp
 1559  unzip raspberry_tcp_openvpn.zip cyberghost_tcp
 1560  unzip raspberry_tcp_openvpn.zip cyberghost_tcp/
 1561  cd cyberghost_tcp/
 1562  unzip ../raspberry_tcp_openvpn.zip
 1563  ls
 1564  cp openvpn.ovpn /etc/openvpn/
 1565  sudo cp openvpn.ovpn /etc/openvpn/
 1566  cd /etc/openvpn/
 1567  ls
 1568  sudo mv openvpn.ovpn CG_CH_TCP.conf
 1569  sudo nano CG_CH.conf
 1570  sudo systemctl restart openvpn
 1571  sudo nano CG_CH.conf
 1572  sudo systemctl restart openvpn
 1573  ls
 1574  sudo CG_CH_TCP.conf
 1575  sudo nano CG_CH_TCP.conf
 1576  ls
 1577  sudo nano user_tcp.txt
 1578  sudo nano /etc/default/openvpn
 1579  sudo reboot
 1580  cd /etc/default/
 1581  ls
 1582  sudo nano openvpn
 1583  sudo reboot
 1584  ifconfig
 1585  nslookup google.com
 1586  cat /etc/resolv.conf
 1587  sudo nano /etc/dhcpcd.conf
 1588  sudo reboot
 1589  cat /etc/resolv.conf
 1590  ifconfig
 1591  cd /etc/
 1592  ls
 1593  cd privoxy/
 1594  ls
 1595  sudo nano config
 1596  cd templates/
 1597  ls
 1598  cd ..
 1599  ls
 1600  sudo nano config
 1601  sudo nano user.action
 1602  sudo nano default.action
 1603  cd ../openvpn/
 1604  ls
 1605  cp CG_CH.conf CG_US.conf
 1606  sudo cp CG_CH.conf CG_US.conf
 1607  sudo nano CG_US.conf
 1608  sudo nano user_us.txt
 1609  sudo nano /etc/default/openvpn
 1610  sudo reboot
 1611  systemctl list-units | grep vpn
 1612  sudo update-rc.d --list
 1613  sudo update-rc.d
 1614  ls /etc/init.d/
 1615  ls -l /etc/init.d/
 1616  cd /etc/init.d/
 1617  sudo nano openvpn
 1618  sudo update-rc.d openvpn remove
 1619  ls -l
 1620  sudo nano openvpn
 1621  sudo reboot
 1622  chkconfig
 1623  sysv-rc-conf
 1624  sudo apt install sysv-rc-conf
 1625  sysv-rc-conf
 1626  sudo sysv-rc-conf
 1627  systemctl list-units | grep vpn
 1628  sudo systemctl disable openvpn
 1629  sudo reboot
 1630  systemctl list-units | grep vpn
 1631  sudo systemctl enable openvpn
 1632  sudo reboot
 1633  dmesg
 1634  apt-cache search firmware wireless
 1635  ifconfig
 1636  sudo apt-get install firmware-realtek
 1637  sudo iwlist scan
 1638  sudo usb-devices
 1639  sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
 1640  lsusb
 1641  iwconfig
 1642  lsmod
 1643  lsmod | grep realtek
 1644  sudo nano /etc/network/interfaces
 1645  cd /etc/network/interfaces.d/
 1646  ls
 1647  iwconfig
 1648  sudo reboot
 1649  git clone -b v5.8.1 https://github.com/fastoe/RTL8811CU_for_Raspbian
 1650  cd RTL8811CU_for_Raspbian
 1651  ls
 1652  make
 1653  sudo make install
 1654  sudo modprobe 8821cu
 1655  sudo reboot
 1656  dmesg
 1657  lsusb
 1658  sudo nano /etc/network/interfaces
 1659  sudo service networking restart
 1660  systemctl status networking.service
 1661  iwconfig
 1662  sudo nano /etc/network/interfaces
 1663  systemctl status networking.service
 1664  sudo service networking restart
 1665  lsmod
 1666  lsmod | grep rtl
 1667  sudo modprobe rtl8811cu
 1668  uname -r
 1669  sudo apt install -y bc git dkms build-essential raspberrypi-kernel-headers
 1670  iwconfig
 1671  ifocnfig
 1672  ifconfig
 1673  sudo nano /etc/dhcpcd.conf
 1674  sudo apt-get update
 1675  sudo apt-get upgrade
 1676  sudo apt install hostapd dnsmasq
 1677  sudo nano /etc/dhcpcd.conf
 1678  sudo mv /etc/dnsmasq.conf /etc/dnsmasq.conf.orig
 1679  sudo nano /etc/dnsmasq.conf
 1680  sudo nano /etc/hostapd/hostapd.conf
 1681  ifconfig
 1682  sudo nano /etc/hostapd/hostapd.conf
 1683  sudo nano /etc/default/hostapd
 1684  sudo reboot
 1685  ifconfig
 1686  sudo nano /etc/dnsmasq.conf
 1687  sudo nano /etc/dhcpcd.conf
 1688  sudo nano /etc/hostapd/hostapd.conf
 1689  sudo nano /etc/default/hostapd
 1690  sudo nano /etc/sysctl.conf
 1691  sudo iptables -t nat -A POSTROUTING -o wlan1 -j MASQUERADE
 1692  sudo sh -c "iptables-save > /etc/iptables.ipv4.nat"
 1693  sudo nano /etc/rc.local
 1694  sudo systemctl stop dnsmasq
 1695  sudo systemctl disable dnsmasq
 1696  sudo apt remove dnsmasq
 1697  sudo apt purge dnsmasq
 1698  sudo apt install bridge-utils
 1699  sudo nano /etc/network/interfaces.d/br0
 1700  sudo systemctl restart networking
 1701  sudo reboot
 1702  ifconfig
 1703  sudo systemctl status hostapd
 1704  sudo nano /etc/hostapd/hostapd.conf
 1705  sudo systemctl unmask hostapd
 1706  sudo systemctl enable hostapd
 1707  sudo systemctl status hostapd
 1708  sudo journalctl -u hostapd
 1709  sudo systemctl restart hostapd
 1710  sudo systemctl status hostapd
 1711  sudo iw dev wlan0 info
 1712  sudo nano /etc/hostapd/hostapd.conf
 1713  sudo systemctl restart hostapd
 1714  sudo nano /etc/hostapd/hostapd.conf
 1715  sudo systemctl restart hostapd
 1716  sudo iw dev wlan0 info
 1717  sudo systemctl status hostapd
 1718  sudo iw dev wlan0 info
 1719  sudo iw dev wlan1 info
 1720  sudo nano /etc/hostapd/hostapd.conf
 1721  sudo systemctl restart hostapd
 1722  sudo iw dev wlan0 info
 1723  sudo nano  sudo systemctl restart hostapd
 1724  sudo nano /etc/network/interfaces
 1725  cd /etc/network/interfaces
 1726  cd /etc/network/interfaces.d/
 1727  ls
 1728  sudo nano br0
 1729  ls
 1730  sudo nano wlan0
 1731  cat /etc/sysctl.conf
 1732  sudo systemctl restart networking
 1733  cd
 1734  sudo nano /etc/hostapd/hostapd.conf
 1735  sudo systemctl restart networking
 1736  sudo iw dev wlan0 info
 1737  lsusb
 1738  cd /
 1739  cd etc
 1740  cd network/
 1741  ls
 1742  sudo nano interfaces
 1743  cd interfaces.d/
 1744  ls
 1745  sudo rm br0
 1746  sudo apt-get remove bridge-utils
 1747  sudo apt-get autoremove
 1748  sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
 1749  sudo sh -c "iptables-save > /etc/iptables.ipv4.nat"
 1750  sudo nano /etc/rc.local
 1751  sudo apt-get install dnsmasq
 1752  sudo systemctl restart dnsmasq
 1753  sudo systemctl restart networking
 1754  sudo nano /etc/dnsmasq.conf
 1755  sudo systemctl start hostapd
 1756  sudo systemctl start dnsmasq
 1757  sudo systemctl enable hostapd
 1758  sudo systemctl enable dnsmasq
 1759  sudo reboot
 1760  iwconfig
 1761  sudo reboot
 1762  sudo nano /etc/dhcpcd.conf
 1763  cd /etc/network/interfaces.d/
 1764  ls
 1765  cat wlan0
 1766  cd ..
 1767  cd interfaces.d/
 1768  sudo rm wlan0
 1769  iwconfig
 1770  sudo systemctl restart networking
 1771  iwconfig
 1772  sudo nano /etc/network/interfaces
 1773  ls
 1774  cd ..
 1775  cd
 1776  sudo nano /etc/hostapd/hostapd.conf
 1777  sudo systemctl restart networking
 1778  iwconfig
 1779  interface=br0
 1780  dhcp-range=192.168.1.100,192.168.1.200,255.255.255.0,24h
 1781  sudo reboot
 1782  sudo nano /etc/rc.local
 1783  sudo nano /etc/hostapd/hostapd.conf
 1784  iwconfig
 1785  cd /etc/hostapd/
 1786  ls
 1787  cp hostapd.conf hostapd.conf.wpa2
 1788  sudo cp hostapd.conf hostapd.conf.wpa2
 1789  sudo nano hostapd.conf
 1790  sudo cp hostapd.conf hostapd.conf.wpa
 1791  sudo nano /etc/default/hostapd
 1792  sudo nano /etc/sysctl.conf
 1793  sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
 1794  sudo iptables -A FORWARD -i eth0 -o wlan0 -m state --state RELATED,ESTABLISHED -j ACCEPT
 1795  sudo iptables -A FORWARD -i wlan0 -o eth0 -j ACCEPT
 1796  sudo iptables -t nat -A POSTROUTING -o wlan1 -j MASQUERADE
 1797  sudo iptables -A FORWARD -i wlan0 -o wlan1 -j ACCEPT
 1798  sudo iptables -A FORWARD -i wlan1 -o wlan0 -m state --state RELATED,ESTABLISHED -j ACCEPT
 1799  sudo sh -c "iptables-save > /etc/iptables.ipv4.nat"
 1800  sudo nano /etc/iptables.ipv4.nat
 1801  sudo reboot
 1802  iwconfig
 1803  sudo nano /etc/network/interfaces
 1804  sudo nano /etc/dhcpcd.conf
 1805  sudo nano /etc/dnsmasq.conf
 1806  iwconfig
 1807  sudo reboot
 1808  iwconfig
 1809  sudo nano sudo nano /etc/dnsmasq.conf
 1810  sudo nano /etc/iptables.ipv4.nat
 1811  sudo iptables -F
 1812  sudo iptables -t nat -F
 1813  sudo iptables -t mangle -F
 1814  sudo iptables -X
 1815  sudo iptables -P INPUT ACCEPT
 1816  sudo iptables -P FORWARD ACCEPT
 1817  sudo iptables -P OUTPUT ACCEPT
 1818  sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
 1819  sudo iptables -A FORWARD -i eth0 -o wlan0 -m state --state RELATED,ESTABLISHED -j ACCEPT
 1820  sudo iptables -A FORWARD -i wlan0 -o eth0 -j ACCEPT
 1821  sudo iptables -F
 1822  sudo iptables -t nat -F
 1823  sudo iptables -t mangle -F
 1824  sudo iptables -X
 1825  sudo iptables -P INPUT ACCEPT
 1826  sudo iptables -P FORWARD ACCEPT
 1827  sudo iptables -P OUTPUT ACCEPT
 1828  sudo iptables -t nat -A POSTROUTING -o wlan1 -j MASQUERADE
 1829  sudo iptables -A FORWARD -i wlan1 -o wlan0 -m state --state RELATED,ESTABLISHED -j ACCEPT
 1830  sudo iptables -A FORWARD -i wlan0 -o wlan1 -j ACCEPT
 1831  sudo sh -c "iptables-save > /etc/iptables.ipv4.nat"
 1832  sudo systemctl enable dnsmasq
 1833  sudo systemctl enable hostapd
 1834  sudo systemctl start dnsmasq
 1835  sudo systemctl start hostapd
 1836  sudo nano /etc/hostapd/hostapd.conf
 1837  sudo reboot
 1838  sudo nano /etc/rc.local
 1839  iwconfig
 1840  sudo nano /etc/dhcpcd.conf
 1841  ifconfig
 1842  sudo nano /etc/privoxy/config
 1843  sudo apt-get remove --auto-remove privoxy
 1844  sudo apt-get purge --auto-remove privoxy
 1845  sudo reboot
 1846  iwconfig
 1847  sudo raspi-config
 1848  sudo nano /etc/dhcpcd.conf
 1849  sudo nano /etc/network/interfaces
 1850  sudo nano /etc/hostapd/hostapd.conf
 1851  sudo nano /etc/dnsmasq.conf
 1852  cd /etc/
 1853  ls
 1854  sudo mv /etc/dnsmasq.conf /etc/dnsmasq.conf.bak
 1855  sudo nano /etc/dnsmasq.conf
 1856  sudo reboot
 1857  raspi/c#
 1858  raspi
 1859  raspi-
 1860  sudo raspi-config
 1861  sudo nano /etc/dhcpcd.conf
 1862  ifconfig
 1863  ipconfig
 1864  iwconfig
 1865  cd /etc/network/
 1866  ls
 1867  cd interfaces.d/
 1868  ls
 1869  cd ..
 1870  sudo nano interfaces
 1871  sudo nano /etc/dhcpcd.conf
 1872  sudo nano /etc/dnsmasq.conf
 1873  ls
 1874  sudo ls
 1875  cd ..
 1876  ls
 1877  mv dnsmasq.conf dnsmasq.conf.bridge
 1878  sudo mv dnsmasq.conf dnsmasq.conf.bridge
 1879  sudo cp dnsmasq.conf.orig  dnsmasq.conf
 1880  sudo nano /etc/dnsmasq.conf
 1881  sudo nano /etc/network/interfaces
 1882  sudo shutdown
 1883  sudo shutdown now
 1884  sudo nano /etc/dhcpcd.conf
 1885  sudo reboot
 1886  iwconfig
 1887  ifconfig
 1888  sudo nano /etc/hostapd/hostapd.conf
 1889  sudo reboot
 1890  iwconfig
 1891  sudo nano /etc/default/hostapd
 1892  sudo nano /etc/hostapd/hostapd.conf
 1893  sudo nano /etc/rc.local
 1894  sudo reboot
 1895  iwconfig
 1896  ipconfig
 1897  ifconfig
 1898  sudo ifconfing wlan0 down
 1899  sudo ifconfig wlan0 down
 1900  sudo ifconfig wlan0 up
 1901  iwconfig
 1902  sudo service hostapd status
 1903  iwconfig
 1904  sudo service hostapd status
 1905  iwconfig
 1906  sudo nano /etc/network/interfaces
 1907  sudo nano /etc/dhcpcd.conf
 1908  sudo service dhcpcd restart
 1909  iwconfig
 1910  ipconfig
 1911  ifconfig
 1912  iwconfig
 1913  sudo reboot
 1914  iwconfig
 1915  sudo service hostapd status
 1916  iwconfig
 1917  sudo reboot
 1918  iwconfig
 1919  sudo nano /etc/rc.local
 1920  sudo reboot
 1921  iwconfig
 1922  ls
 1923  nano start_squeezelite.sh
 1924  sudo reboot
 1925  sudo nano /etc/rc.local
 1926  sudo reboot
 1927  iwconfig
 1928  nano start_squeezelite.sh
 1929  sudo reboot
 1930  iwconfig
 1931  ifconfig
 1932  iwconfig
 1933  iwconfig wlan0
 1934  dsmeg | grep wlan0
 1935  dmesg | grep wlan0
 1936  sudo nano /etc/hostapd/hostapd.conf
 1937  sudo reboot
 1938  iwconfig
 1939  ifconfig
 1940  sudo nano /etc/sysctl.conf
 1941  sudo nano /etc/rc.local
 1942  sudo nano /etc/iptables.ipv4.nat
 1943  sudo nano /etc/rc.local
 1944  sudo rm  /etc/iptables.ipv4.nat
 1945  sudo reboot
 1946  sudo iptables -F
 1947  sudo iptables -X
 1948  sudo iptables -Z
 1949  sudo iptables -t nat -F
 1950  sudo iptables -t nat -X
 1951  sudo iptables -t nat -Z
 1952  sudo iptables -t mangle -F
 1953  sudo iptables -t mangle -X
 1954  sudo iptables -t mangle -Z
 1955  sudo iptables -P INPUT ACCEPT
 1956  sudo iptables -P OUTPUT ACCEPT
 1957  sudo iptables -P FORWARD ACCEPT
 1958  sudo iptables -t nat -A POSTROUTING -o tun0 -j MASQUERADE
 1959  sudo iptables -A FORWARD -i tun0 -o wlan0 -m state --state RELATED,ESTABLISHED -j ACCEPT
 1960  sudo iptables -A FORWARD -i wlan0 -o tun0 -j ACCEPT
 1961  sudo apt-get install iptables-persistent
 1962  sudo nano /etc/default/openvpn
 1963  sudo reboot
 1964  resolvconf -l interface-tun0
 1965  resolvconf -l interface tun0
 1966  ip route show table all
 1967  cd /etc/openvpn
 1968  ls
 1969  sudo nano CG_CH.conf
 1970  sudo reboot
 1971  cat /etc/resolv.conf
 1972  ifconfig
 1973  ifconfig /all
 1974  ifconfig /a
 1975  route
 1976  cat /etc/resolv.conf
 1977  nslookup bing.com 10.3.4.1
 1978  ping 10.3.4.73
 1979  ping 10.3.4.1
 1980  nslookup bing.com 10.3.4.1ip route show table all
 1981  ip route show table all
 1982  resolvconf -l
