# Networking

Here is a description of my network.

I have an ISP-provided router. Unfortunately I cannot put it into bridge mode, so it provides Network Address Translation (NAT) to devices behind it. However, this may work to my advantage for something I have planned for the [future](/docs/issues/plans.md).

Behind this, I have a tower: Lenovo ThinkCentre M710t i3 7100 3.90 GHz with 16 GB of RAM, a 256 GB SSD and a 500 GB HDD. This has two NICs; one, the onboard NIC that came with the tower, and the other, a TP-LINK TG-3468 Gigabit PCI Express Network Adapter. I purchased this tower off of eBay for less than $100 (shipping included!).

From the tower, I have an unmanaged switch connected (NetGear GS105). I would like to eventually replace this with a managed switch; not least because a few of the LEDs where the ethernet cables connect already died within a few months of the purchase, but also just so I can learn more about managed switches.

Directly connected to the unmanaged switch is the Fedora 43 workstation, the Raspberry Pi, and the TP-LINK WAP.

The TP-LINK WAP is an AC1200 Wireless Dual Band Router Model No. Archer A5. I have disabled the 2.4GHz functionality.

For all devices using SSH, I choose a non-traditional port and still only allow access to SSH from LAN. The only ports exposed to the Internet are for WireGuard, http/https, and Jellyfin, with the latter two using a reverse proxy to the Raspberry Pi.

The tower's OS is Proxmox, a Debian-based hypervisor OS. You can read more details about the Proxmox setup in [hypervisor](/docs/software/hypervisor.md).

The first VM is OPNsense. OPNsense is the network's central focus point. It is the network's router, gateway, firewall, and more. It is the only node on the network that is NAT'd by the ISP-provided router, receiving a static IP address from the router, but to the rest of the LAN, it is [OPNsense's LAN IP]. Here are the in-depth details:

![image](/assets/photos/opnsense_dashboard.png)

- WAN is on vtnet0
- LAN is on vtnet1
- HTTP Strict Transport Security enabled
- SSH enabled but not for root user (admin user created)
## Reverse Proxy:
- I installed Caddy (os-caddy plugin) and set up the following firewall rules:
    - WAN: Allow http traffic from anywhere to this firewall
    - WAN: Allow https traffic from anywhere to this firewall
    - LAN: Allow http traffic from anywhere to this firewall
    - LAN: Allow https traffic from anywhere to this firewall
- Auto https on
- I set up domains and handlers in the Caddy interface to ensure that I am able to access Nextcloud, Jellyfin, and use my carddav/caldav items, which are located in Nextcloud. When I first did this, there was a lot of trial and error, as the information I was able to find online can be sporadic and sometimes contradictory.
- I had to put a header in there for Strict Transport Security.

![image](/assets/photos/opnsense_caddy.png)

## Firewall Rules
- I have a few firewall rules based on my needs.
- Block LAN from accessing ISP-provided router
    - I don't want any device to be able to access this; if I need to be able to, I will connect directly via Ethernet to it.
- Block Roomba from Internet
    - I don't want my Roomba to send information about my home back to any company; I value my privacy, and I believe that once a product is paid for, the transaction is finished and I no longer owe anything to the company that made the product. I do not use the Roomba app (except for when I initially set it up to connect to my network's WiFi) and control it via openHAB.
- Allow Roomba to only communicate with Raspberry Pi (two rules, one for blocking access to LAN, one for passing access to Pi)
    - The Raspberry Pi has openHAB running on it, so the Roomba must communicate with it.

![image](/assets/photos/opnsense_firewall_LAN.png)

## WireGuard VPN
- I want to keep my mobile devices connected to the home network at all times, so as soon as they disconnect from the home WiFi they connect to the VPN. Setting up the VPN requires a lot more than just turning it off and on:
    - Create WireGuard instance
    - Firewall rules:
        - Allow WireGuard devices to access network
        - Allow WireGuard devices to access OPNsense GUI
        - Allow traffic over 51820 to firewall
        - Allow traffic over 51820 to access Internet
    - Generate a peer (mobile device)
    - Import peer info to mobile device

![image](/assets/photos/opnsense_VPN.png)

## IDS (Suricata)
- Due to using VirtIO in Proxmox, Suricata cannot run as an IPS in OPNsense. Eventually I will have a dedicated device to run OPNsense on bare metal, and then I can simply run Suricata as an IDS/IPS, but for now it is only an IDS.
- I had to disable hardware offloading features per recommendation from OPNsense's documentation.
- The rulesets I chose:
    - abuse.ch/Feodo Tracker
    - abuse.ch/ThreatFox
    - abuse.ch/SSL IP Blacklist
    - abuse.ch/SSL Fingerprint Blacklist
    - abuse.ch/URLHaus
    - ET open/botcc
    - ET open/compromised
    - ET open/dshield

![image](/assets/photos/opnsense_IDS.png)