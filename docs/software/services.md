# Services

Here's a long list of the software that I use and what I use it for. I'll try to exclude common stuff (LibreOffice, etc.) and focus on stuff that might be of interest in a homelab setting.

I went over all the stuff installed on the Raspberry Pi in [servers](/docs/hardware/servers.md), so this will focus more on Fedora workstation and the smartphone.

## Fedora

- btop
    - It just looks cooler than top or htop
- Dino
    - For text messages that come over my VoIP numbers
- Guake
    - Another thing that just looks cool instead of a regular boring terminal
- Gradia
    - For editing screenshots
- KeepassXC
    - Password locker (database stored in Nextcloud)
- Kooha
    - For screen recording
- Librewolf
    - More private/secure version of Firefox (which is already more private/secure than its competitors)
- LocalSend
    - For quick file sharing between devices on the network
- OpenSnitch
    - Application firewall for outgoing connections
- Remote Viewer
    - For accessing GUIs of VMs via SPICE (or headless VMs during the initial install stage)
- Ungoogled Chromium
    - For when something just straight up doesn't work in Librewolf
- Veracrypt
    - For encrypting files or entire disks (like my backups!)
- Virtual Machine Manager
    - I don't really run VMs off of my workstation anymore, but I have it here just in case
- Visual Studio Code
    - It's just really good for writing the contents of this github and for learning python (still in the beginner stages of that)
- Wireshark
    - Occasionally I'll analyze my network traffic with Wireshark; I'm hoping one day when I have a powerful enough server to just have it constantly analyze all network traffic via tshark

## Smartphone

(security-related apps were listed in [overview](/docs/security/overview.md))

Pretty much everything below is FOSS.

- Accrescent
    - App store for FOSS apps
- Android2Linux
    - Receive Android notifications on my Linux computer
- Cheogram
    - To manage my VoIP phone numbers
- ConnectBot
    - Connect to my devices via SSH
- DAVx5
    - For syncing with my contacts hosted in Nextcloud
- F-Droid
    - App store for FOSS apps; can add repos
- FindMyDevice
    - FOSS app that lets me send commands to my phone using a PIN from another phone in case it gets lost (take a picture, turn on location, etc.)
- ICSx5
    - For syncing with my calendars hosted in Nextcloud
- Island
    - To manage the work profile
- Keep Alive
    - If I don't use the smartphone at all for a certain number of hours, an emergency text gets sent to people I have specified
- Keepass2Android
    - For KeepassXC
- LocalSend
    - For quick file sharing between devices on the network
- MakeACopy
    - Take pictures of documents and the app does its best to make them look like scanned documents.
- MDM Agent
    - I plan to set up Headwind MDM on my Debian server at some point to practice using mobile device management; I have this on here to remind me not to forget to do it eventually.
- Nextcloud (including Nextcloud Notes and Nextcloud Bookmarks)
    - No need to explain this one
- ntfy
    - Get alerts sent from various servers instantly about a variety of things
- OpenKeychain
    - Keeps my PGP keys for Thunderbird
- SambaLite
    - To access Samba shares on my Windows devices
- ServerBox
    - To monitor servers on my network from my phone
- Syncthing
    - To sync certain folders on my phone with Nextcloud directly