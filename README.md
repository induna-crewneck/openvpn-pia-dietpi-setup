# Setting up OpenVPN with PrivateInternetAccess on DietPi
### Description
I spent two days trying to set this up correctly and wanted to share this to TTmaybe save someone else some work.
I used [KiTTY](https://dietpi.com/downloads/binaries/all/Kitty_Portable_DietPi.7z) to control the Pi's console and FileZilla to push/pull files. If you're unsure how to use SHH or FTP look that up first, we will need to access files and enter commands so figure out a way to do that before proceeding.

### Necessary Software
Make sure you have openvpn, resolvconf and dnsmasq installed and running
```
apt-get install openvpn
apt-get install dnsmasq
apt-get install openresolv
```
If you're asked if you want to uninstall something, answer n

![Softwareinstall](https://i.imgur.com/qMz33GE.png)

### Download, edit and push necessary files
#### Download
If you want to get the files freshly from their source to make sure they are up to date and safe, contunue. I also uploaded my files [here](/Files) which you can use as they are not personalized. If you use my files you can skip to [Push](#push)

Download and unpack the [DEFAULT](https://www.privateinternetaccess.com/openvpn/openvpn.zip) or [STRONG](https://www.privateinternetaccess.com/openvpn/openvpn-strong.zip) Configuration Files from PIA. I used PIA's 4096-bit OpenVPN configuration files. If you want to use the default files replace every instance of 4096 in the code with 2048. 

Download the resolvconf Update script from [here](https://github.com/alfredopalhares/openvpn-update-resolv-conf).


#### Edit
Pick the server that you want to use. For this I will use the US East server. Rename the ovpn file of the server you chose to change the file extension from .ovpn to .conf and replace any spaces with underscores. So "US East.ovpn" becomes "US_East.conf"

Open the file with a text editor. There should be a line saying "auth-user-pass" and there may be lines saying "ca" and "crl-verify" but there may not be. Just make sure these lines are added/edited to say:
```
ca /etc/openvpn/ca.rsa.4096.crt
auth-user-pass /etc/openvpn/login
crl-verify /etc/openvpn/crl.rsa.4096.pem
```
also add these lines to the very end of the file:
```
script-security 2
up /etc/openvpn/update-resolv-conf.sh
down /etc/openvpn/update-resolv-conf.sh
down-pre
```
#### Push
We need to copy 4 files in total to /etc/openvpn/ on the Pi:
```
ca.rsa.4096.crt
crl.rsa.4096.pem 
US_East.conf
update-resolv-conf.sh
```
