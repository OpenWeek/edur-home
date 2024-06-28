# edur-home: Deploy eduroam @ home

Easily deploy an `eduroam` connection at your place and use your institutional credentials to log in.

The edur-home project was developed during UCLouvain's [Open Week](https://uclouvain.be/en/research-institutes/icteam/ingi/open-week.html), edition 2024, by @nicojmn, @TheoTechnicguy, and Matteo. The idea is to provide students with preconfigured routers they can put in their dorms and have access to the internet without complex setup.

The entire project is based on [OpenWRT](https://openwrt.org) (version 23.05.3) for the router firmware. The RADIUS server configurations are for [FreeRADIUS](https://freeradius.org/), both on OpenWRT and Debian for proxying requests. The [Debian](https://www.debian.org/) proxying server is a virtual private server running version 6.1.90-1.

## Project setup

The all necessary RADIUS files are located in [`radius-config`](radius-config/). `radius-config/openwrt` contains explanations and configuration for setting up the RADIUS server on an OpenWRT router. `radius-config/proxy` explains the setup for an `eduroam` Service Provider relay server.

## Getting Started

You will most likely want to broadcast and `eduroam` WLAN. Check this [OpenWRT README](radius-config/openwrt/README.md) for guidance. If, instead, you want to become a Service Provider, check the requirements with your country's national RADIUS proxy server provider and have a look at this [README](radius-config/proxy/README.md).

