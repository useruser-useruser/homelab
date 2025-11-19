# Firewalls

For the network firewall, see the firewall section in [networking](/docs/network/networking.md)

On all of my Debian-based devices, I use ufw for the device firewall. Redhat-based devices use firewalld. I only open ports for services I use often or should always be running (SSH, NFS, etc.); otherwise, I use an SSH tunnel.

On my Fedora workstation, I use OpenSnitch as an application firewall. Every time I try to use an application to access something outside of the workstation, it asks me if I want to do that and tells me what IP it is trying to get to and over which port. For obvious things I set it to always allow, but I try not to do this for everything. It blocks access until I click allow.