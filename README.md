# Install arch-linux
Setup a new installation of Arch Linux.

This playbook is intended to run against an arch-linux live iso
to bootstrap an "fresh" arch-linux system as basis for further 
configurations. This play is *not idempotent*.

It automates most of the steps from the 
[Installation guide](https://wiki.archlinux.org/index.php/Installation_guide).
Some steps are left out because the aim of this play is to give you
a mostly clean system that works as target for further ansible deployments.


## Pre requirements
You should have booted a [arch-linux live system](https://www.archlinux.org/download/)
where root can login via ssh.


## How To
- boot [arch-linux live system](https://www.archlinux.org/download/)
- set root's password using `passwd`
- start ssh server with `systemctl start sshd`
- (optional) copy your ssh key via `ssh-copy-id -i KEY root@target`
- copy `hosts.sample` to `hosts` and adjust to your needs
- run `ansible-playbook site.yml` (add `-k` if you didn't copy your ssh key)


## Actions done by the play
- prepare disk (msdos label, one partition, no swap)
- create a ranked mirrorlist (configurable, default 'DE')
- bootstrap arch-linux (base + sshd and python for ansible)
- enable remote ssh login for root (copy password and authorized_keys from live system)
- reboot into new installed arch-linux
- set LANG, generate locale and set timezone ('en_US.UTF-8' and 'Europe/Berlin')
- (optional) setup qemu-guest-agent
- (optional) install additional os packages