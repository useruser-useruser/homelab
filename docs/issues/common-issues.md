# Common Issues

I run into quite a few issues, but here are the ones that have stuck with me and will take further troubleshooting to eventually solve.

- The Lenovo tower is essentially a hardware limitation. It only has 16 GB of RAM and isn't the most powerful device, though it has done a lot for my homelab. It's just not designed for what I use it for.
    - I try to set swappiness very low (down to zero sometimes) so that my SSDs don't degrade quickly. However, this has caused Proxmox to force VMs to shut down. I ended up limiting the amount of memory from 12 GB to 6 GB for both OPNsense and Debian; to be fair, OPNsense doesn't really need a whole lot at the moment since I'm not running an IPS on it. However, I can't even get the Graylog Docker container to even pull without the VM crashing when I have swappiness set so low, so I had to just set it back to 60 and hope for the best. It worked then, but I'm still having issues getting Graylog working, so getting a proper SIEM set up is a work in progress.

- Since OPNsense is run inside a VM and uses VirtIO for the NICs, I can't use Suricata as an IPS. I plan to get around this by running ZenArmor while Suricata acts as an IDS. However, once I have a dedicated device just for OPNsense, it'll all just be Suricata.

- When I was using both 5GHz and 2.4GHz on the TP-LINK WAP, Kismet was able to detect the 2.4GHz SSID even though I set it not to broadcast on either band. I don't use 2.4GHz anymore, but I never could figure that one out.

- With my commercial VPN on the Fedora workstation, WireGuard just never works. I have to always set it to OpenVPN (UDP).

- Sometimes OPNsense will just not let you log in anymore if you're using MFA (I know how to use its quirky way of inputting MFA) and so you have to SSH in to try and figure out why. Sometimes you can and sometimes you can't. When you can't, you have to reinstall. I'm going to mitigate this by beginning a backup/snapshot cycle.

- The same issue happens with Proxmox sometimes too, also when using MFA. For that I just have to set it back up, and in the future use the VM backups that I'll have stored on a separate device.