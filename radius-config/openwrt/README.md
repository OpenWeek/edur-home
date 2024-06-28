# RADIUS Configurations: OpenWRT

This directory contains the RADIUS configuration files for OpenWRT (and other) routers.
These configurations files should be saved at the radius configuration root (`/etc/freeradius3/` on OpenWRT)

## Firmware configuration

First thing you need to do is to go on [the OpenWRT website](https://openwrt.org/) and download the firmware for your router.
We strongly recommend to check the [OpenWRT firmware selector](https://firmware-selector.openwrt.org/) to see if your
router is supported, and select the right firmware for your device.

### Image selection

You have 2 options following your router state:

- If your router is already running OpenWRT, download the sysupgrade binary.
- If your router is running the stock firmware, you need to download the factory binary.

After downloading the firmware, check the file integrity with the sha256sum provided on the website.

### Flashing the firmware

If you are flashing from the stock firmware, fetch your manufacturer's documentation to see how to flash the firmware.
If you already have OpenWRT installed on your router follow these steps to upgrade your firmware:

1. Power off your router.
2. Plug in an **ethernet cable** (not the power cable yet !) on the lan port of your router.
3. Hold down the reset button on your router.
4. **While holding the reset button**, plug in the power cable.
5. Wait for the indicator light(s) to blink.
6. Release the reset button.
7. Add a static IP address to the pc ethernet interface (e.g. `192.168.1.2`)

   ```bash
   ip addr add 192.168.1.2/24 dev <interface>
   ```

8. Connect to [192.168.1.1](http://192.168.1.1) with your browser.
9. Go on the `System` tab, then [`Backup / Flash Firmware`](http://192.168.1.1/cgi-bin/luci/admin/system/flash).
10. Upload the file you downloaded and click on the flash image button.
11. Wait for the router to reboot / reload the page and connect with root/root as creds.

## FreeRADIUS on the router

### Installation

To install FreeRADIUS on OpenWRT, you need to connect to your router with SSH and run the following commands (make sure
you have an internet connection), optionally, you can install the `freeradius3-utils` package for additional testing tools.

```bash
opkg update && opkg install freeradius3
opkg install freeradius3-utils # Optional
```

In order for eduroam to work, you need to install the `wpad` package, which is not installed by default. You will
maybe need to remove preinstalled WPA related packages to avoid conflicts.

```bash
opkg install wpa2-eap
```

### Configuration

On the OpenWRT interface, go to the `Network` tab, then `wireless` and add a new wireless network with the following settings:

1. General setup tab
   - Mode: `Access Point`
   - ESSID: `eduroam`
   - Network: `lan`
   - leave the rest as default

2. Wireless Security tab
   - Encryption: `WPA2-EAP`
   - RADIUS Authentification Server : `127.0.0.1`
   - RADIUS Authentification Port : `1812`
   - RADIUS Authentification Secret : `your_secret` (the same as in the `clients.conf` file)
   - leave the rest as default
  

#### Testing the configuration

To test the configuration, you can use the `radclient` command from the `freeradius3-utils` package.
First, you need to create a test user in the `/etc/freeradius3/mods-config/files/authorize` file, you can just simply
uncomment the `bob` user.

```bash
echo "User-Name=bob, User-Password=hello" | radclient -x localhost auth testing123
```

If the command returns `Access-Accept`, your configuration is correct, otherwise check the logs in `/var/log/radius/radius.log`.
