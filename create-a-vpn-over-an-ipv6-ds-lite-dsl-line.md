Create a VPN over an IPv6 DS-LITE DSL line

I recently moved to a new apartment and switched from a cable line back to a DSL line. For years I have used different versions of [Fritz Box][fritz], a router from AVM.

#### My setting so far consists of:

`Raspberry PI <--> Fritz Box <--> Public network`  
`Local IPv4   <--> Fritz Box <--> Public IPv4`  
`Local net    <-->    VPN    <--> Public network`  

I had an external, public IPv4 address which was mapped by [a dynamic DNS service][NO-IP], so I could access the VPN server running on my Fritz Box. 

Unfortunately that setup [doesn't work][avm] anymore. Theoretically it would work, if the public network uses an IPv6 address range. Most public WiFi's still use IPv4, so no connection can be established from there. 
I find it especially annoying that to that date the 3G/4G data networks from Vodafone and O2 [do not use the IPv6 (GER)][4G] network. 

_So I could not access my home server._


`Raspberry PI <--> Fritz Box <--> Public network`  
`Local IPv4   <--> Fritz Box <--> Public IPv6`  
`Local net    <-->    VPN    <-!> Public network`  

#### My setup now

```
Raspberry PI <-->   Fritz Box   <--> Public network  
Local IPv4   <-->   Fritz Box   <--> Public IPv6  
Local net    <-->   wireguard   <--> streisand server  
```  
And:   
```
Laptop       <--> Public network <--> streisand server
```

Wireguard? [Wireguard!][wireguard]

To quote their website: 
> WireGuard® is an extremely simple yet fast and modern VPN that utilizes state-of-the-art cryptography. It aims to be faster, simpler, leaner, and more useful than IPSec, while avoiding the massive headache.

It offers the possibility for clients to talk to each other. 

For some time now I use a [streisand VPN][streisand]. It set ups several connection endpoints, one of them is a wireguard server. Streisand offers the possibility to create several profiles. 

I now connect from the Raspberry PI to the wireguard server.
On my laptop I also connect to the wireguard server. 

Now I can connect to the Raspberry PI from the laptop.

#### setting up a streisand VPN

Please follow the official instructions for the installation. You'll need a public server (like an Amazon EC2). Streisand automatically set's uo the whole server. After the installation you'll get instructions to open the VPN site, log in and download profiles.

#### setting up the wireguard client on the Raspberry

1. Install the Rapsberry PI headers (necessary for the DKMS module)

 `sudo apt-get install raspberrypi-kernel-headers`

2. Follow the instructions from the [official site][wireguard_install]
3. Download the profile for the wireguard client from the streisand VPN site
4. Follow the installation instructions for the profile from the streisand VPN site. You don't need to change the defaults.

#### setting up the wireguard client on your laptop (or other endpoint)

Follow the official instructions, depending on your distribution/OS.

_make sure to choose another profile for the wireguard client!_

#### test your setup

1. Start the wireguard connection from the Raspberry to the streisand/wireguard server
 
 `sudo wg-quick up wg0-client`

2. Start the wireguard connection from your laptop to the streisand/wireguard server

 `sudo wg-quick up wg0-client`

3. connect to your server:

 `ssh pi@LocalWireguardIp`

 _Your LocalWireguardIp is written down in your downloaded profile_

#### conclusion

I could have bought an IPv4 option with my ISP and circumventing all of those problems. Maybe there is even a workaround for an IPv4->IPv6 tunnel to connect to the FritzBox VPN server. 

Since I already had a running streisand I tried the above approach. 

If you have remarks or questions I'll be happy to read your mail! 


[fritz]: https://en.avm.de/products/fritzbox/
[NO-IP]: https://www.noip.com 
[avm]: https://en.avm.de/service/fritzbox/fritzbox-7430/knowledge-base/publication/show/1497_Can-MyFRITZ-be-used-on-an-internet-connection-with-IPv6-DS-Lite/
[4G]: https://www.heise.de/newsticker/meldung/Vodafone-IPv6-im-Mobilfunknetz-kommt-aber-nicht-im-August-4119195.html
[wireguard]: https://www.wireguard.com/
[streisand]: https://github.com/StreisandEffect/streisand
[wireguard_install]: https://www.wireguard.com/install/

Tags: vpn, ipv6, fritzbox, ds-lite, wireguard, streisand
