# easylist-pac-privoxy
EasyList Tracker and Adblock Rules to Proxy Auto Configuration (PAC) File and Privoxy Actions and Filters

Converts [EasyList](https://easylist.to/index.html) tracker and ad blocking rules to efficient filter blocks in a [proxy.pac](https://raw.githubusercontent.com/essandess/easylist-pac-privoxy/master/proxy.pac) file for automatic proxy network configurations and [Privoxy](http://www.privoxy.org) proxy servers.

## Proxy Auto Configuration (PAC)

### To Use: GitHub Host

Set your network Proxy Auto Configuration setting to:

> `https://raw.githubusercontent.com/essandess/easylist-pac-privoxy/master/proxy.pac`

***Advantages***

* Works on any mobile or desktop device on any WiFi network worldwide.
* GitHub server; private web server not necessary.

***Disadvantages***

* Does not work on mobile data networks.
* Reliance on a third-party (me) for pass/block rule sets, updates, and `proxy.pac` integrity.

### To Use: Localhost

Download the [proxy.pac](https://raw.githubusercontent.com/essandess/easylist-pac-privoxy/master/proxy.pac) file.

On macOS (without Server.app):

```
sudo cp ~/Downloads/proxy.pac /Library/WebServer/Documents
sudo apachectl start
```

Set your network Proxy Auto Configuration setting to:

> `http://localhost/proxy.pac` or `http://host-ip-address/proxy.pac`

***Advantages***

* Works for any mobile or desktop device on your LAN.
* Works with an upstream proxy if specified in the `proxy.pac` file.
* Individual update control and customization of the `proxy.pac` file and filter rules.
* Possible internet access if port 80 exposed outside the LAN firewall.

***Disadvantages***

* Does not work on mobile data networks.
* No internet access unless port forwarding to host is used.

### To Use: VPN

Configure an [OpenVPN](../../../essandess/osx-openvpn-server) to use the `proxy.pac` file hosted on your LAN.

This is the best option.

***Advantages***

* Works on any mobile or desktop device on any mobile data or WiFi network worldwide.
* Individual update control and customization of the `proxy.pac` file and filter rules.
* Security and privacy benefits of VPNs.

***Disadvantages***

* Necessity of VPN server.

## Details

Using EasyList rules in a in a [proxy.pac](https://raw.githubusercontent.com/essandess/easylist-pac-privoxy/master/proxy.pac) file provides these benefits:

* Tracker and Ad blocking performed in all clients that use PAC files, browsers and non-browsers alike.
* Tracker and Ad blocking on both desktop and mobile devices, especially via [VPN](../../../essandess/osx-openvpn-server).
* Browser plugins or filtering proxies are not necessarily used (although PAC files work well in sequence with these).
* PAC files do not alter the webpage DOM, used by adblock detection methods.

The script `easylist_pac.py` downloads EasyList and EasyPrivacy [rules](https://adblockplus.org/filter-cheatsheet) and converts these to a combination of very efficient Javascript hash lookups and efficient NFA regular expressions. The size of the PAC file and rulesets are limited in the posted example to a total of over fifteen thousand (18788) to ensure efficient execution on modern mobile devices. For full rulesets, use in conjunction with a browser plugin and/or Privoxy. 

Example hash (exact match) blocking entries look like:

```
"tracker.myseofriend.net"
"adwiretracker.fwix.com"
```

Example regular expression blocking rules look like:

```
online.*/promoredirect?key=
secureprovide1.com/*=tracking
```

### EasyList to `proxy.pac` converter

```
python3 easylist_pac.py
python3 easylist_pac.py -h
python3 easylist_pac.py -b blackhole-ip-address:port -d download_dir -p proxy:port -P proxy.pac.orig
```

The new file [proxy.pac](https://raw.githubusercontent.com/essandess/easylist-pac-privoxy/master/proxy.pac) will be created in the (default `~/Downloads` directory. See `easylist_pac.py -h` for options.

## Privoxy

Under development. See [here](../../../../skroll/privoxy-adblock/issues/11) for related issues.
