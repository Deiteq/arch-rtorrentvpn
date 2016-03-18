**Application**

[rTorrent](https://github.com/rakshasa/rtorrent)
[ruTorrent](https://github.com/Novik/ruTorrent)
[OpenVPN](https://openvpn.net/)
[Privoxy](http://www.privoxy.org/)

**Description**

rTorrent is a quick and efficient BitTorrent client that uses, and is in development alongside, the libTorrent (not to be confused with libtorrent-rasterbar) library. It is written in C++ and uses the ncurses programming library, which means it uses a text user interface. When combined with a terminal multiplexer (e.g. GNU Screen or Tmux) and Secure Shell, it becomes a convenient remote BitTorrent client. This Docker image includes the popular ruTorrent web frontend to rTorrent for ease of use, as well as OpenVPN to ensure a secure and private connection to the Internet, including use of iptables to prevent IP leakage when the tunnel is down. Privoxy is also included to allow unfiltered access to index sites, to use Privoxy please point your application at http://<host ip>:8118.

**Build notes**

Latest stable rTorrent release from Arch Linux.
Latest stable ruTorrent release from Arch Linux AUR using Packer to compile.
Latest stable OpenVPN release from Arch Linux repo.
Latest stable Privoxy release from Arch Linux repo.

**Usage**
```
docker run -d \
    --cap-add=NET_ADMIN \
    -p 9080:9080 \
    -p 9443:9443 \
    -p 8118:8118 \
    --name=<container name> \
    -v <path for data files>:/data \
    -v <path for config files>:/config \
    -v /etc/localtime:/etc/localtime:ro \
    -e VPN_ENABLED=<yes|no> \
    -e VPN_USER=<vpn username> \
    -e VPN_PASS=<vpn password> \
    -e VPN_REMOTE=<vpn remote gateway> \
    -e VPN_PORT=<vpn remote port> \
    -e VPN_PROTOCOL=<vpn remote protocol> \
    -e VPN_PROV=<pia|airvpn|custom> \
    -e ENABLE_PRIVOXY=<yes|no> \
    -e LAN_NETWORK=<lan ipv4 network>/<cidr notation> \
    -e DEBUG=<true|false> \
    -e PUID=<uid for user> \
    -e PGID=<gid for user> \
    binhex/arch-rtorrentvpn
```

Please replace all user variables in the above command defined by <> with the correct values.

**Access application**

`http://<host ip>:9080/`

or

`https://<host ip>:9443/`

Username:- admin
Password:- rutorrent

**Access Privoxy**

`http://<host ip>:8118`

**PIA provider**

PIA users will need to supply VPN_USER and VPN_PASS, optionally define VPN_REMOTE 
(list of gateways https://www.privateinternetaccess.com/pages/client-support/#signup) 
if you wish to use another remote gateway other than the Netherlands.

**PIA example**
```
docker run -d \
    --cap-add=NET_ADMIN \
    -p 9080:9080 \
    -p 9443:9443 \
    -p 8118:8118 \
    --name=rtorrentvpn \
    -v /root/docker/data:/data \
    -v /root/docker/config:/config \
    -v /etc/localtime:/etc/localtime:ro \
    -e VPN_ENABLED=yes \
    -e VPN_USER=myusername \
    -e VPN_PASS=mypassword \
    -e VPN_REMOTE=nl.privateinternetaccess.com \
    -e VPN_PORT=1194 \
    -e VPN_PROTOCOL=udp \
    -e VPN_PROV=pia \
    -e ENABLE_PRIVOXY=yes \
    -e LAN_NETWORK=192.168.1.0/24 \
    -e DEBUG=false \
    -e PUID=0 \
    -e PGID=0 \
    binhex/arch-rtorrentvpn
```

**AirVPN provider**

AirVPN users will need to generate a unique OpenVPN configuration
file by using the following link https://airvpn.org/generator/

1. Please select Linux and then choose the country you want to connect to
2. Save the ovpn file to somewhere safe
3. Start the delugevpn docker to create the folder structure
4. Stop delugevpn docker and copy the saved ovpn file to the /config/openvpn/ folder on the host
5. Start delugevpn docker
6. Check supervisor.log to make sure you are connected to the tunnel

**AirVPN example**
```
docker run -d \
    --cap-add=NET_ADMIN \
    -p 9080:9080 \
    -p 9443:9443 \
    -p 8118:8118 \
    --name=rtorrentvpn \
    -v /root/docker/data:/data \
    -v /root/docker/config:/config \
    -v /etc/localtime:/etc/localtime:ro \
    -e VPN_ENABLED=yes \
    -e VPN_PROV=airvpn \
    -e ENABLE_PRIVOXY=yes \
    -e LAN_NETWORK=192.168.1.0/24 \
    -e DEBUG=false \
    -e PUID=0 \
    -e PGID=0 \
    binhex/arch-rtorrentvpn
```

**Notes**

User ID (PUID) and Group ID (PGID) can be found by issuing the following command for the user you want to run the container as:-

```
id <username>
```
___
If you appreciate my work, then please consider buying me a beer  :D

[![PayPal donation](https://www.paypal.com/en_US/i/btn/btn_donate_SM.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=MM5E27UX6AUU4)

[Support forum](http://lime-technology.com/forum/index.php?topic=45822.0)