# MultiTech
Information about CSN MultiTech LoRaWAN gateway configuration.

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
