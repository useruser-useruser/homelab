# Networking

Here is a description of my network; see the diagram [here]().

I have an ISP-provided router. Unfortunately I cannot put it into bridge mode, so it provides Network Address Translation (NAT) to devices behind it. However, this may work to my advantage for something I have planned for the [future](/docs/issues/plans.md).

Behind this, I have a tower: Lenovo ThinkCentre M710t i3 7100 3.90 GHz with 16 GB of RAM, a 256 GB SSD and a 500 GB HDD. This has two NICs; one, the onboard NIC that came with the tower, and the other, a TP-LINK TG-3468 Gigabit PCI Express Network Adapter. I purchased this tower off of eBay for less than $100 (shipping included!).

From the tower, I have an unmanaged switch connected (NetGear GS105). I would like to eventually replace this with a managed switch; not least because a few of the LEDs where the ethernet cables connect already died within a few months of the purchase, but also just so I can learn more about managed switches.

Directly connected to the unmanaged switch is the Fedora 43 workstation, the Raspberry Pi, and the TP-LINK WAP.

The TP-LINK WAP is an AC1200 Wireless Dual Band Router Model No. Archer A5. I have disabled the 2.4GHz functionality.

The tower's OS is Proxmox, a Debian-based hypervisor OS. You can read more details about the Proxmox setup in [hypervisor](/docs/software/hypervisor.md). 