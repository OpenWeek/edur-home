# RADIUS Configurations: Service Provider proxy

This guide explains how to become a `eduroam` Service Provider. Unless you are an institution, you will most likely want the [other guide](../openwrt/README.md).

## How `eduroam` authentication works

`eduroam` makes use of the domain name to know where to forward the request. The hierarchical structure is similar to the workings of a DODAG tree. When a server has information about treating a request with the specified domain, it proxies it to that server or authentication module. Otherwise, it forwards it to it's parent.

<!-- TODO: wait for credit policy -->
<!-- ![The hierarchical structure of RADIUS](https://www.eduroam.be/node/files/radiushierachies.png) -->
<!-- > Image from <https://eduroam.be/> -->

## Contact your national RADIUS proxy server provider

First, determine what orgnistaion is responsible for your country's national RADIUS proxy server. You will have to contact them to discuss details about feasibility and requirements, as well as transmit your future proxy's IP address and a shared secret.

For Belgium, this organisation is [Belnet](https://belnet.be/).

## Creating your proxy server

FreeRADIUS is available as a [package for Debian](https://packages.debian.org/bookworm/freeradius) and as [OCI image](https://hub.docker.com/r/freeradius/freeradius-server). The IP address for the server must be a static one, as you will need to share it to your national RADIUS proxy server.

To configure this server, use the configuration files available in this direcrtory and check the resources below.

## Included in the configuration

The configuration in this directory are the minimum required to run an operational proxy.

- The [`proxy.conf`](proxy.conf) specifies the national RADIUS proxy. 
    > :point_right: You need to change this file.
- The [`eduroam`](sites-available/eduroam) virtual server confgures the request proxying.
- The [`clients.conf`](clients.conf) specifies the client configuration (IP address and shared secret).
    > :point_right: You need to change this file.

## Resources

- Belnet's RADIUS hierarchy page: <https://www.eduroam.be/node/13>
- Belnet's *How to join* page: <https://www.eduroam.be/node/4>
- Belnet's eduraom configuration page: <https://www.eduroam.be/node/7>
- GEANT's eduroam wiki page for on-site setup: <https://wiki.geant.org/pages/viewpage.action?pageId=121346259>
