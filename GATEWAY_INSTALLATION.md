# Multitech Gateway Installation onto the Cambridge Sensor Network

## General setup

These gateways should all be registered to the main CSN TTN account with gateway IDs like `csn-mtcap-014abe`, `csn-mtcdtip-003c6f` etc (i.e. "csn-" + the product code of the gateway + the last 12 bits of the LoRa node address). Currently we're pointing them directly at the `ttn-router-eu` router (not using a local relay). They use DHCP to obtain their IP addresses wherever they are connected, so the local computer officer needs to be provided with the ethernet MAC address of the gateways to register them in their DHCP server.

## Configuring a new Multitech LoraWAN gateway 

Before starting, collect essential settings information from an existing production server 
in `tfc_prod/csn/secrets`.

### Initial setup

1. Get a stand-alone ethernet switch, no WAN connectivity needed.
2. Connect the Multitech gateway to the switch and power it up.  The gateway with start with IP 192.168.2.1 and 
run a *DHCP server* which will serve addresses in the 192.168.2.X range.
3. Connect a laptop or other workstation configured to accept DHCP addresses to the same switch.  Alternatively
you can configure the workstation to use manual settings and use 192.168.2.100, 255.255.255.0

When making changes to the MultiTech gateways, the changes are only applied once the gateways have been rebooted.

These instructions apply to gateways shipped with 1.6.x firmware.

1. First boot.
    - Browse to https://192.168.2.1/ and continue despite the certificate warning. We can configure a real certificate later.
    - The default credentials to log in to the web interface are admin/admin
    - A wizard will appear. We will accept most of the defaults.
    - On the time zone configuration section, set the time zone to Europe/London
    - On the network settings page, add the correct information for the gateway address and DNS servers. For the Huawei network this should be 192.168.2.254 for the gateway, and Google's DNS servers 8.8.8.8 and 4.4.4.4
    - The gateway is configured to run a DHCP **server** by default. Disable this.
    - Reboot
2. Second boot
    - Update the firmware on the gateway. The new firmware is available from the [MultiTech Downloads](http://www.multitech.net/developer/downloads/) pages. For the metal gateways, use "Conduit: AEP Model w/Node-RED", for the plastic Coduit AP use "Conduit Access Point: AEP Model (MTCAP)"
    - Note that every time the firmware on the devices is updated, the packet forwarder will need to be reintalled (if a custom packet forwarder is being used).
    - Reboot
3. Third boot
    - Configure the gateway in [Device HQ](https://www.devicehq.com/)
    - From the gateway's web interface, select `Administration` -> `Remote Management`
    - Enter our key from Device HQ web site.
    - Switch auto update frequency to 240
4. Fourth boot
    - By default, the gateway is configured to use US915 frequencies. To switch it back to EU868 log in to the web interface.
    - Select `Setup` -> `LoRa Network Server`.
    - Temporarily enable `Network Server`, set frequency plan to EU868, and save. Then disable the network server.
    - Do the same for the `Packet Forwarder`.
5. Fifth boot
    - We will switch to using Jac Kersing's packet forwarder. Instructions for doing this may be found on [TTN's web site about AEP MultiTech Conduits](https://www.thethingsnetwork.org/docs/gateways/multitech/aep.html).
    - In the TTN console, register a new Gateway ID.
    - SSH to the IP address of the LoRaWAN gateway and log in with admin credentials.
    - Run `wget https://github.com/kersing/multitech-installer/raw/master/installer.sh`
    - Run `sh installer.sh`
    - Enter appropriate details
6. Sixth boot
    - Set admin password
7. Seventh boot
    - Configure networking
8. Eighth boot
    - Save default config
