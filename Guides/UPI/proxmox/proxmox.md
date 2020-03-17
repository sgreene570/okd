This guide just adds some extra steps needed to complete an OKD 4 installation
on Proxmox VE. Follow the libvirt instructions to complete a cluster installation,
but refer to these steps for creating and setting up the cluster VM's instead.
Skip the instructions for setting up the internal libvirt network.

[Base Documentation](../libvirt/libvirt.md)

## Creating VM's

Add the latest Fedora CoreOS Bare Metal Live ISO to your install ISO registry.

Download the Fedora CoreOS live ISO file from the [Fedora CoreOS Downloads Page](https://getfedora.org/en/coreos/download/).
(Click on "Bare Metal & Virtualized" tab, first download link under "Bare Metal" section).

Proxmox VM creation does not support the addition of boot time kernel arguments.

That being said, create 3 master, 2 worker, and 1 bootstrap VM's through the proxmox
web UI, selecting the Fedora CoreOS live ISO as your installation media.


## Powering on the VM's

Since the VM's won't run ignition on first boot, use the proxmox web console VNC
to copy over the proper ignition file and run coreos-installer.

Example (auto logged in as in the Core user):

```bash
scp root@okd4-bastion.okd4:~/ign/master.ign .
sudo coreos-installer install /dev/sda -i master.ign
```

Note: Instead of scp'ing the config from the bastion, could also wget it from a web server, or copy the text over manually into a new file.

Note: /dev/sda might not be the proper volume, run `lsblk` or similair to identify install target.

Once this step is completed, the node can be powered off.

Once you have run coreos-installer on each of the VM's with the proper ignition file, and powered them off,
start all of the VM's at once to start the cluster installation.


