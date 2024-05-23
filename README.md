# Install Arch Linux
Setup a new installation of Arch Linux.

This playbook is intended to run against an arch linux live iso
to bootstrap an "fresh" arch linux system as basis for further 
configurations. This play is **not idempotent**.

It automates most of the steps from the 
[Installation guide](https://wiki.archlinux.org/index.php/Installation_guide).
Some steps are left out because the aim of this play is to give you
a mostly clean system that works as target for further ansible deployments.


## Pre requirements
You should have booted a [arch linux live system](https://www.archlinux.org/download/)
where root can login via ssh.
Ensure that the IP address doesn't change when rebooting; Or login and add the IP address from inventory.


## HowTo
- boot [arch linux live system](https://www.archlinux.org/download/)
- set root's password using `passwd`
- check disk and ensure install partition has enough space
- start ssh server with `systemctl start sshd`
- (optional but recommended) copy your ssh key via `ssh-copy-id -i KEY root@target`
- copy the inventory `host.sample.yaml` to `host.yaml` and adjust to your needs
- run `ansible-playbook site.yml` (add `-k` if you didn't copy your ssh key)
- hint: check and set known IP address if play hangs at 'reboot'

## Actions done by the playbook
Best way get an idea of the tasks of the playbook is to read `site.yml`
itself. But here the main steps:
 
- prepare disk (msdos label, one partition, no swap, `/etc/fstab`)
- create a ranked mirrorlist (country configurable, default 'DE')
- bootstrap arch-linux (base, linux kernel, sshd and python for ansible)
- enable remote ssh login for root (copy password and `authorized_keys` from live system)
- Network stuff (`/etc/hostname`, `/etc/hosts`, `systemd-networkd`, `systemd-resolved`)
- reboot into new installed arch linux
- set LANG, generate locale and set timezone (defaults are 'en_US.UTF-8' and 'Europe/Berlin')
- (optional) setup qemu-guest-agent
- (optional) install additional os packages
