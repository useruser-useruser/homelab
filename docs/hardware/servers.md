# Servers

I have three servers at the moment:
    - Raspberry Pi 4 running Debian 13 (Raspberry Pi OS)
    - VM running Debian 13
    - VM running Windows Server 2022

I haven't started using Windows Server; I only installed it.

The two servers run Debian 13 because every few months I like to tear everything down and set it back up, and most recently did this in early November 2025, soon after Raspberry Pi OS updated its base to Debian 13.

## Raspberry Pi

The Raspberry Pi primarily serves as a cloud storage server and media server. I purchased an external 256 GB SSD as using a microSD card would not be very useful in this regard (though when I was first learning everything it was capable of, I did only use this).

The Raspberry Pi is also what keeps my FQDN correctly pointed at my network; my ISP provides me with my own IP address, but it is dynamic. I use a very cheap domain name provider which has their own tool to keep them updated on my IP address. I installed this on the Raspberry Pi, and it simply broadcasts my IP address back to them every five minutes.

As with all of my devices, SSH keys are required to access it, but I also add MFA (6-digit OTP) to SSH for added protection.

### Nextcloud

For cloud storage, I set up Nextcloud. When I was first learning, I used NextcloudPi, but that did not work as well as I wanted. I tried to use Docker, but I had issues getting the Docker container to work correctly (and back then I did not have much experience with Docker). In the end I went with just installing it on bare metal, and have done that ever since, every time I do a rebuild. I build a simple database using MariaDB:

    CREATE DATABASE [name of the database];
    CREATE USER '[name of the database user]'@'localhost' IDENTIFIED BY '<PASSWORD>';
    GRANT ALL PRIVILEGES ON [name of the database].* TO '[name of the database user]'@'localhost';
    FLUSH PRIVILEGES;

Fortunately that's the simple part. I won't cover all of the steps, just the major ones.

I also set up an Apache web server and create a virtual host file in /etc/apache2/sites-available:

    <VirtualHost *:80>
      ServerName [FQDN]
      ServerAlias www.[FQDN]
      DocumentRoot /var/www/[FQDN]/public_html
      ErrorLog ${APACHE_LOG_DIR}/[FQDN]_error.log
      CustomLog ${APACHE_LOG_DIR}/[FQDN]_access.log combined
    </VirtualHost>

I create a nextcloud.conf file in /etc/apache/sites-available as well:

    Alias /nextcloud "/var/www/[FQDN]/nextcloud/"

    <Directory /var/www/[FQDN]/nextcloud/>
    Require all granted
    AllowOverride All
    Options FollowSymLinks MultiViews

    <IfModule mod_dav.c>
        Dav off
    </IfModule>

    </Directory>

Once I have done the above, I access it in a web browser and continue the setup. I add the plugins that I want (and deactivate the ones I don't) before syncing anything, because I need to make sure that everything is encrypted as it is added. I use Nextcloud's "Default Encryption Module" plugin. I make the necessary changes concerning memory limit and other small items to /etc/php/8.4/apache2/php.ini and /etc/php/8.4/cli/php.ini. I sync everything in the Nextcloud folder on my main workstation; I do this while connected via the local Raspberry Pi address so I'm not sending all of my data over the Internet just to come back; I know it's all encrypted, but it's just way faster to first load it all up over LAN, and then establish on my workstation that the account is reached over the Internet via my FQDN and not over the local network. I do this out of habit, as I used to take my main workstation out of the home often, but it's so old I can't really do that anymore. I have to make around a dozen small adjustments to things to get them to work, but I have built my own manual that has every single step outlined so I can simply copy and paste; one of my goals is to learn python so I can automate this entire thing. 

One of the first things I do is set up MFA since this is exposed to the Internet.

I set up Syncthing on my phone to keep my Gallery and Download folder synced to Nextcloud. I create a syncthing folder in /opt/, sync the phone to that, and then add it as external storage in Nextcloud. This keeps items synced in the background; it is not as fast as using LocalSend when I just want to quickly send something between my phone and workstation.

### Jellyfin

For the media server, I use Jellyfin (bare metal). I know a lot of people use Plex, but I liked the simplicity and FOSS ethos of Jellyfin that I didn't see in Plex. I have a large collection of physical media that I have obtained over the years, and have worked to digitize all of it. It is all stored on two hard drives (one SSD and one HDD) that have been joind via LVM and are sitting in a docking station next to the Raspberry Pi. The docking station is connected to the Proxmox tower and is accessed by the Raspberry Pi as NFS and kept permanently mounted (with a line in fstab) on /mnt/. I plan to eventually purchase several more hard drives and create a RAID setup, as I will be using the same storage for SIEM logs, and I don't want the digitized media to suddenly disappear.

The difficult part with Jellyfin was figuring out how to get the Caddy reverse proxy to properly point at it with just adding /jellyfin to the end of the FQDN. This involved setting up two handlers in OPNsense with the following information:

    Options	Values
    Domain:	[FQDN]
    Path	/jellyfin/*
    Directive	reverse_proxy
    Protocol:	http://
    Upstream Domain:	[local IP for Pi]
    Upstream Port:	8096
    Description	jellyfin reverse proxy

and

    Options	Values
    Domain:	[FQDN]
    Path	/jellyfin
    Directive	reverse_proxy
    Protocol:	http://
    Upstream Domain:	[local IP for Pi]
    Upstream Port:	8096
    Description	jellyfin reverse proxy

This took a lot of back and forth, web searching, and trial and error. But it works!

### Others

I use Webmin as my dashboard to check what is going on; I make sure to set up MFA with it, even though it's not accessible from the Internet.

I also use NetData as a more in-depth dashboard, though it lags a bit so I only use it when I really need to drill down deep into an issue.

I have Stirling PDF installed via Docker for PDF editing.

I have Omni-tools installed via Docker because it has a lot of useful small tools.

I have NetAlertX installed via Docker so that I can quickly see if any new devices are on my LAN. I do hide the SSID of the WAP, and make the password EXTREMELY long, which I know isn't fool-proof, but so far nobody besides myself or my spouse has accessed the LAN, and I can see it clearly in NetAlertX. I have set it up so that if any new devices do access it that aren't mine, I get an alert sent to my phone via ntfy immediately.

I installed Kismet (and had to purchase a small WiFi dongle since Raspberry Pi's built-in WiFi card doesn't support monitor mode). This is more just to satisfy my curiosity and see what wireless devices are around me. I also want to make sure my WAP's SSID isn't being broadcast (it was when I had the 2.4GHz on, but not anymore since I turned that off). Eventually I would like to get enough devices to create a miniature network to practice penetration testing on, but that is far down the road.

I have Uptime Kuma installed on the Raspberry Pi to warn me when certain services go down. I should probably install this on a separate device, though, because usually when a service goes down it's because the entire Pi has locked up or cannot be reached, which means Uptime Kuma cannot notify me of anything. It uses ntfy to send messages to my phone.

I have Crowdsec installed as kind of a web application firewall; so far it hasn't had to block anything, which either means my network is well hidden or Crowdsec isn't doing anything. I assume the former since Crowdsec seems to be a good tool for many people.

I used to have openHAB installed because of the Roomba that I own, but I stopped using the Roomba and openHAB was always hogging a ton of resources on the Pi. When I move to my next home and get a PoE security camera I'll reinstall openHAB (hopefully it runs better in Debian 13).

## Debian

This server is a VM in Proxmox. Originally I wanted the Raspberry Pi to act as a vulnerability scanner for the network, but it could not handle it. After purchasing this tower and setting up Proxmox, I created the Debian VM (I chose Debian because that's what I have the most experience with, and it's very easy to use once you know how to use it). I installed OpenVAS Greenbone via Docker and Nessus Essentials via deb. I run scans about once a week; most of the results are items of no consequence or something that my risk analysis considers not likely to happen to me. One of the big challenges was finding a way to allow them to do an authenticated scan on the Raspberry Pi without being able to put in a 6-digit OTP for MFA. I had to create a new user on the Pi that has very limited functions but does not need MFA for SSH (still uses keys). It was a little complex, but I figured it out.

I really want this VM to be a SIEM as well, but I keep running into issues when setting up Graylog. I think the main issue is just that the tower doesn't have enough RAM. It only has 16 GB, which must be split between three 24/7 OSes (Proxmox, OPNsense, Debian) and then when I want to use one of the other non-permanent VMs I have installed on there. I don't want to go out and spend $1000 on a new tower just for homelab practice, so once the AI bubble pops, I'll buy some RAM on the cheap and upgrade. I believe this tower maxes out at 64 GB, so that's probably what I'll get.