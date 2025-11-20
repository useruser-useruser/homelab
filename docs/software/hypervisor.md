# Hypervisor

I use Proxmox as my hypervisor on the Lenovo Tower (see [hardware overview](/docs/hardware/overview.md)).

![image](/assets/photos/proxmox.png)

*(Two of the VMs are missing because I recently deleted them.)*

I chose Promox because originally the tower only had OPNsense installed, but dedicating the entire machine to OPNsense was a bit overkill, and I wanted to have a server for vulnerability scanning and SIEM. My experience before Proxmox with VMs was limited to just having a VM of Windows on my Ubuntu or Fedora workstation using Virtualbox. However, ever since using Proxmox, I have learned a great deal and see the many benefits of virtualizing (especially when an OS needs to be spun up very quickly for specific tasks).

I created an admin user so I don't have to use the root account.

I don't want Promox to have Internet access for security reasons, but it needs to be able to get package updates. Therefore, after setting it up and installing both the OPNsense and Debian VMs, I did the following:

1. No CIDR or gateway for vmbr0
2. CIDR but no gateway for vmbr1
3. Install apt-cacher-ng on Debian VM
4. On Debian, ufw allow [Proxmox IP] to any port 3142 proto tcp
5. On Proxmox, add a repo file to apt.conf.d with:
        
        Acquire::http::Proxy "http://[Debian VM IP]:3142";
        Acquire::https::Proxy "false";

I'll share the settings for the Debian VM so you can get an idea for the kinds of settings I typically use (though keep in mind the Debian VM runs 24/7; most of the VMs only run when I need them):

- Graphic card: SPICE (just so much easier to use)
- Machine: q35
- BIOS: OVMF (UEFI)
- Add EFI disk
- EFI storage: local-lvm (the ones I don't use often are all stored on the HDD)
- Pre-enroll keys: no
- SCSI controller: VirtIO SCSI single
- Qemu agent: yes
- Bus/Device: VirtIO Block
- Storage: local-lvm (ditto above)
- Disk Size: 150 (most of the others are much smaller than this)
- Cache: write through
- Discard: yes
- IO thread: yes
- CPU cores: 3 (had it at 4 before, but ran into [issues](/docs/issues/common-issues.md))
- VCPUs: 3
- aes on
- RAM: 6144 (had it at 12288 but ran into [issues](/docs/issues/common-issues.md))
- Ballooning device: yes
- Bridge: vmbr1
- Model: VirtIO (paravirtualized)
- Multiqueue: 1

The two hard drives that I LVMd together into one that sit in a docking station are connected to the tower and run through Proxmox. They just work better that way than before when I had them directly connected to the Raspberry Pi, even though Jellyfin is installed on the Pi.