# MultiTech
Information about Cambridge Sensor Network MultiTech LoRaWAN gateway configuration.

## Types of gateway

We currently have three types of MultiTech LoRaWAN gateway. All types have onboard copper ethernet ports.

<img src="https://www.multitech.com/documents/media/images/products/multiconnect/conduit/mt_multiconnect_conduit_clear_main.png" width="400">

The small blue metal gateway is a *MultiConnect® Conduit®* (**MTCDT**) gateway which supports two line cards (one of which is a LoRaWAN card). We only have one of these, and we currently aren't using the second line card.

<img src="https://www.multitech.com/documents/media/images/products/multiconnect/conduit/ip67/MT_MultiConnect_Conduit_IP67_Base_Station_9-2017.png" width="400">

The large white metal gateway with multiple antennae is a *MultiConnect® Conduit® IP67 Base Station* (**MTCDTIP**) which is like the blue gateway in an IP67 housing. We have five of these and they are all equipped with LoRaWAN and cellular line cards.

<img src="https://www.multitech.com/documents/media/images/products/multiconnect/conduit-ap/MT_MultiConnect_Conduit_AP_Color_Logo_clear_hr1000px.png" width="400">

The small white plastic gateway is a *MultiConnect® Conduit® AP* (**MTCAP**). It has no line cards, but onboard LoRaWAN. We have two right now.

## General setup

These gateways should all be registered to the main CSN TTN account with gateway IDs like `csn-mtcap-014abe`, `csn-mtcdtip-003c6f` etc (i.e. "csn-" + the product code of the gateway + the last 12 bits of the LoRa node address). Currently we're pointing them directly at the `ttn-router-eu` router (not using a local relay). They use DHCP to obtain their IP addresses wherever they are connected, so the local computer officer needs to be provided with the ethernet MAC address of the gateways to register them in their DHCP server.

## From box to deployed

When the LoRaWAN gateways come from the distributor, they are configured to have the static IP address of 192.168.2.1. With the AEP gateways we use, a DHCP client is also enabled by default, with the AEP gateway falling back to a static address after 20 seconds.

We use a Huawei E5770 router to provide a network connection between our setup PC, the LoRaWAN gateway, and the internet. This router presents a LAN which allows clients such as our setup PC to obtain an address by DHCP within 192.168.2.64/24-192.168.2.128/24 with internet gateway address 192.168.2.254.

We configure the LoRaWAN gateways using a web browser and an SSH client.

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
