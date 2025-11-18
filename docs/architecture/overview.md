# System Architecture Overview

## Router:
- OPNsense
    - All roads lead here. This is actually run as a VM inside Proxmox; one day when I can purchase a dedicated device (preferably https://eu.protectli.com/product/v1211/), OPNsense will have a home of its own.
    - Due to hardware limitations, it is allowed 3 cores and 6 GB of RAM.
    - OPNsense acts as my network's:
        - [gateway](/docs/network/networking.md)
        - [router](/docs/network/networking.md)
        - [firewall](/docs/security/firewall.md)
        - [DNS resolver](/docs/network/networking.md) (Unbound)
        - [DNS sinkhole](/docs/network/networking.md) (Unbound)
        - [reverse proxy](/docs/network/networking.md) (Caddy)
        - [IDS](/docs/security/other.md) (Suricata)
        - [VPN terminus](/docs/network/networking.md) (WireGuard)
    - I have plans to utilize ZenArmor as an IPS (I cannot use Suricata as the IPS since I am using VirtIO). I will update here when I have done that.

## Hypervisor:
- Proxmox
    - This hosts OPNsense, Debian 13 (server), Windows Server 2022, Windows 10, Ubuntu, Kali, and Rocky. The device it is on is a cheap, older tower that only has 16 GB of RAM, which severely limits what I can do, BUT the good news is I opened it up the other day and discovered there is an entire 500 GB HDD just sitting in there unused; I had an extra SATA cable and connected it and voila, the VMs I don't use as often can be stored on that, saving me from going out and buying a new hard drive.
    - More info [here](/docs/software/hypervisor.md)

## Wireless Access Point:
- TP-Link WAP
    - This could be a router, but I set it to Access Point mode. It has 2.4GHz and 5GHz, but I only keep the 5GHz on (when I used the Roomba before, I would turn the 2.4GHz on).

## Physical Servers:
- Raspberry Pi 4
    - This got me started on my homelab journey back in 2021. I really just wanted to wean myself off of cloud storage that I had no control over (Google Drive, Dropbox, OneDrive, etc.), so I learned about Nextcloud and bought this. For awhile I pushed it beyond its limits, trying to install everything possible, but once I learned what it can and cannot do, I settled on a few major and a bigger handful of minor tools. The two biggest purposes this serves for me are:
        - Cloud storage (Nextcloud)
        - Media server (Jellyfin)
    - More info in [containers](/docs/software/containers.md) and [services](/docs/software/services.md).
    - More info about the Pi itself in [servers](/docs/hardware/servers.md).

## Virtual Servers:
- Debian 13
    - Due to hardware limitations, I am limiting this to 6 GB of RAM and 3 cores. I've dedicated 150 GB of storage space to it.
    - Right now this is serving as my network's vulnerability scanner. However, I really want to set this up as my network's SIEM as well, but I have had [issues](/docs/issues/common-issues.md). The vulnerability scanners I use are Greenbone and Nessus Essentials.
- Windows Server 2022
    - I only recently downloaded and installed this and have not yet had time to tinker around with it. I really want to learn more about it because a lot of organizations still use Windows Server since it integrates well with their Windows workstations.
- More info about both in [servers](/docs/hardware/servers.md).

## Physical Workstations:
- Fedora 43
    - This laptop is my primary workstation. I use this for about 90% of my work. The computer itself is a little bit older and has so many issues, but it has stuck with me for nearly a decade and not given up yet, so I won't give up on it. I can't really carry it around anymore, so it sits at my desk; I'm afraid to move it. I'm even writing all the documentation in VS Code and merging to github from this laptop.
    - More info in [overview](/docs/hardware/overview.md)
- macOS Monterrey
    - I bought this a few months ago because a coworker was selling it for $100; I wanted to learn more about how macOS works, and so far I've learned a good bit from a user's perspective. I now use it as my outside-of-the-home laptop (which only accounts for about 10% because I almost never leave the house except to go to my job). It has to run Monterrey because it's older and Intel-based.
    - More info in [overview](/docs/hardware/overview.md)
- Windows 11
    - This is actually my spouse's computer. She lets me use it whenever I need to use Windows that runs faster than a VM.
    - More info in [overview](/docs/hardware/overview.md)

## Virtual Workstations:
- Kali Linux
    - I have this because I want to be super hacker man. Just kidding; I'm actually hoping for a future job in cybersecurity, and Kali is a great tool for that, whether you're blue or red. I will be complete with my MS in Cybersecurity Technology in December 2025; once I have transitioned to a new job and am living in my next location, I will be focusing more on practicing with Kali.
- Rocky Linux
    - I have this because even though I use Fedora extensively, I know that RHEL has more things that I need to learn if I want to have a job using RHEL. Therefore I have Rocky just to learn RHEL.
- Ubuntu Linux
    - I have this for keeping up my practice using Ubuntu. Though I do use Debian daily with my servers, Ubuntu has some special things (especially in the GUI) that I like to mess around with to stay fresh.
- Windows 10
    - I still have the Windows 10 license from my laptop, so why waste it? Even though Windows 10 has technically reached EOL, I can still learn a lot about Windows by messing with this in a VM; plus there are a few software applications that still don't run well in Linux, even under Wine (looking at you, Tableau Desktop). While I do use Windows 11 at my job every day, I don't want to mess around with a workstation from my job and break something there.

## Miscellaneous:
- Unmanaged switch
- Two hard drives (one SSD, one HDD) that used to be in my laptop and are now in a docking station, combined via LVM. They served as storage for my media files for Jellyfin.
- ISP-provided router (OPNsense NAT'd behind this)
- Smartphone running GrapheneOS, connected in several ways to devices in homelab
- iPad Mini purchased in 2011, still not sure what to do with this extremely outdated item
- Roomba purchased in 2021, was previously connected via openHAB but I haven't used it in awhile and won't take it with me when I move