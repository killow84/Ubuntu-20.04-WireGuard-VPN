# Ubuntu-20.04-WireGuard-VPN
Ubuntu 20.04 set up WireGuard VPN server
----------------------------
# Step 1 – Update your system
sudo apt update
sudo apt upgrade

# Step 2 – Installing a WireGuard VPN server on Ubuntu 20.04 LTS
sudo apt install wireguard

# Step 3 – Configuring WireGuard server
sudo -i
mkdir -m 0700 /etc/wireguard/
cd /etc/wireguard/
ls -l privatekey publickey
cat privatekey
cat publickey
---------------------------------------------------
# Set Up WireGuard VPN on Ubuntu by Editing wg0.conf
# Edit or update the /etc/wireguard/wg0.conf file as follows:
sudo nano /etc/wireguard/wg0.conf
or
sudo vim /etc/wireguard/wg0.conf
Append the following config directives:

## Set Up WireGuard VPN on Ubuntu By Editing/Creating wg0.conf File ##
[Interface]
## My VPN server private IP address ##
Address = 192.168.6.1/24
 
## My VPN server port ##
ListenPort = 41194
 
## VPN server's private key i.e. /etc/wireguard/privatekey ##
PrivateKey = eEvqkSJVw/7cGUEcJXmeHiNFDLBGOz8GpScshecvNHU=
-------------------------------------------------------
Save and close the file when using vim text editor
-------------------------------------------------------
# Step 4 – Set up UFW firewall rules to open required ports
sudo ufw allow 41194/udp
sudo ufw status
--------------------------------------------------------
# Step 5 – Enable and start WireGuard service
sudo systemctl enable wg-quick@wg0
sudo systemctl start wg-quick@wg0
sudo systemctl status wg-quick@wg0
sudo wg
sudo ip a show wg0
--------------------------------------------------------
# Step 6 – Wireguard VPN client configuration
sudo apt install wireguard
sudo sh -c 'umask 077; touch /etc/wireguard/wg0.conf'
sudo -i
cd /etc/wireguard/
umask 077; wg genkey | tee privatekey | wg pubkey > publickey
ls -l publickey privatekey
cat privatekey
Edit the /etc/wireguard/wg0.conf file on your client side:
sudo nano /etc/wireguard/wg0.conf
## OR ##
sudo vim /etc/wireguard/wg0.conf
Append the following directives:
---------------------------------------------------------
[Interface]
## This Desktop/client's private key ##
PrivateKey = uJPzgCQ6WNlAUp3s5rabE/EVt1qYh3Ym01sx6oJI0V4=
 
## Client ip address ##
Address = 192.168.6.2/24
 
[Peer]
## Ubuntu 20.04 server public key ##
PublicKey = qdjdqh2+N3DEMDUDRob8K3b+9BZFJbT59f+rBrl99zM
 
## set ACL ##
AllowedIPs = 192.168.6.0/24
 
## Your Ubuntu 20.04 LTS server's public IPv4/IPv6 address and port ##
Endpoint = 172.105.112.120:41194
 
##  Key connection alive ##
PersistentKeepalive = 15
------------------------------------------------------------
# Enable and start VPN client/peer connection, run:
sudo systemctl enable wg-quick@wg0
sudo systemctl start wg-quick@wg0
sudo systemctl status wg-quick@wg0
sudo systemctl stop wg-quick@wg0
------------------------------------------------------------
sudo vim /etc/wireguard/wg0.conf
Append the following config:
------------------------------------------------------------
[Peer]
## Desktop/client VPN public key ##
PublicKey = u2ao8GNNUWAirtjq0eL1UpHVkMep5/EUalbZcdH0imc=
 
## client VPN IP address (note  the /32 subnet) ##
AllowedIPs = 192.168.6.2/32
------------------------------------------------------------
Save and close the file. Next start the service again, run:
sudo systemctl start wg-quick@wg0

# Step 7 – Verification
ping -c 4 192.168.6.1
sudo wg
