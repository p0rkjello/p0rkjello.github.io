---
layout: post
title: 'Packer CentOS Virtualbox image' 
author: 'Andrew Bounds'
tags:
- linux
- packer
- virtualbox
- vagrant
---

## SATA Wanted

Many of the public [Vagrant boxes](https://app.vagrantup.com/boxes/search) are configured with an IDE controller. I assume this is for compatibility reasons. The SATA controller on VirtualBox performs better, so that is what I want.

***[Serial ATA (SATA)](https://www.virtualbox.org/manual/ch05.html#harddiskcontrollers)***

>Like a real SATA controller, Oracle VM VirtualBox's virtual SATA controller operates faster and also consumes fewer CPU resources than the virtual IDE controller. Also, this enables you to connect up to 30 virtual hard disks to one machine instead of just three, when compared to the Oracle VM VirtualBox IDE controller with a DVD drive attached.

## Getting started

To start the process of building your Vagrant box using Packer. Create a working directory. Within that directory start the [Packer template](#packer-json-file). The template which I named centos-7.7-x86_64.json`  is a file that describes how to build the image.

## Packer json file

Within the `json` file we define the builder type [virtualbox-iso](https://www.packer.io/docs/builders/virtualbox-iso.html). This builder will create a new image from an ISO file. Here is where we can define the __hard_drive_interface__ as SCSI.

> hard_drive_interface (string) - The type of controller that the primary hard drive is attached to, defaults to ide. When set to sata, the drive is attached to an AHCI SATA controller. When set to scsi, the drive is attached to an LsiLogic SCSI controller.

{% raw %}

```json
{
  "builders": [
    {
      "type": "virtualbox-iso",
      "guest_os_type": "RedHat_64",
      "iso_url": "{{user `mirror`}}/7/isos/x86_64/CentOS-7-x86_64-NetInstall-1908.iso",
      "iso_checksum": "{{user `iso_checksum`}}",
      "iso_checksum_type": "{{user `iso_checksum_type`}}",
      "output_directory": "output-centos-7.7-x86_64-{{build_type}}",
      "vm_name": "packer-centos-7.7-x86_64",
      "disk_size": "{{user `disk_size`}}",
      "hard_drive_discard": "{{user `hard_drive_discard`}}",
      "hard_drive_nonrotational": "{{user `hard_drive_nonrotational`}}",
      "headless": "{{user `headless`}}",
      "hard_drive_interface": "{{user `hard_drive_interface`}}",
      "http_directory": "http",
      "boot_wait": "5s",
      "boot_command": [
        "<esc>",
        "<wait>",
        "linux inst.ks=http://{{.HTTPIP}}:{{.HTTPPort}}/ks.cfg biosdevname=0 net.ifnames=0",
        "<enter>"
      ],
      "ssh_timeout": "{{user `ssh_timeout`}}",
      "ssh_username": "vagrant",
      "ssh_password": "vagrant",
      "ssh_pty": true,
      "shutdown_command": "sudo systemctl poweroff",
      "vboxmanage": [
        ["modifyvm", "{{.Name}}", "--memory", "{{user `memory`}}"],
        ["modifyvm", "{{.Name}}", "--cpus", "{{user `cpus`}}"],
        ["modifyvm", "{{.Name}}", "--nic1", "nat", "--nictype1", "virtio"]
      ]
    }
  ],
  "provisioners": [
    {
      "type": "shell",
      "scripts": [
        "scripts/sshd.sh",
        "scripts/virtualbox.sh",
        "scripts/cleanup.sh"
      ]
    }
  ],
  "post-processors": [
    {
      "type": "vagrant",
      "compression_level": "{{user `compression_level`}}",
      "output": "centos-7.7-x86_64-{{.Provider}}.box"
    }
  ],
  "variables": {
    "compression_level": "6",
    "cpus": "1",
    "disk_size": "8000",
    "hard_drive_discard": "true",
    "hard_drive_interface": "sata",
    "hard_drive_nonrotational": "true",
    "headless": "true",
    "iso_checksum": "6ffa7ad44e8716e4cd6a5c3a85ba5675a935fc0448c260f43b12311356ba85ad",
    "iso_checksum_type": "sha256",
    "memory": "512",
    "mirror": "http://mirror.lug.udel.edu/pub/centos",
    "ssh_timeout": "60m"
  }
}
```

{% endraw %}

## CentOS kickstart file

```conf
# RHEL7 - Vagrant lab system
# Install OS instead of upgrade
install
# Keyboard layouts
keyboard 'us'
# After installation reboot
reboot
# Root password
rootpw --plaintext vagrant
# Use text mode install
text
firstboot --disable
# Setup network interface
network --device=eth0 --bootproto=dhcp --onboot=yes --activate
# Install from an installation tree on a remote server
# Required when using a miniml ISO
url --mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os
# Set repo to mirror.centos.org
repo --name="CentOS" --baseurl=http://mirror.centos.org/centos/$releasever/os/$basearch/ --cost=100
repo --name="Updates" --baseurl=http://mirror.centos.org/centos/$releasever/updates/$basearch/ --cost=100
repo --name="epel" --baseurl=https://download.fedoraproject.org/pub/epel/7/x86_64/ --cost=100
# System language
lang en_US.UTF-8
# Logging
logging --level=debug
# System timezone
timezone --utc Etc/UTC
# Firewall
firewall --disable
# SELinux
selinux --permissive
# Accept EULA
eula --agreed
# Services
services --enabled=ntpd,ntpdate
# Add Vagrant user
user --name=vagrant --plaintext  --password=vagrant --groups=vagrant,wheel
# System bootloader configuration
bootloader --location=mbr
# Clear the Master Boot Record
zerombr
# Partition clearing information
clearpart --all --initlabel
# Automatically create partition, LVM
autopart --type=lvm

%packages --ignoremissing --excludedocs
@core
epel-release
dkms
kernel-devel
kernel-headers
make
automake
perl
gcc
gcc-c++
bzip2
which
wget
ntp
ntpdate
# mandatory packages in the @core group
-btrfs-progs
-iprutils
-kexec-tools
-plymouth
# default packages in the @core group
-*-firmware
-dracut-config-rescue
-kernel-tools
-libsysfs
-microcode_ctl
-NetworkManager*
-postfix
-rdma

%end

%post
# Apply Vagrant public key.
echo "Applying Vagrant public key"
sudo mkdir -p /home/vagrant/.ssh
sudo curl -fsSLo /home/vagrant/.ssh/authorized_keys https://raw.githubusercontent.com/hashicorp/vagrant/master/keys/vagrant.pub

# Change owner and group on SSH authorized keys file for vagrant user
sudo chown -R vagrant:vagrant /home/vagrant/.ssh
sudo chmod 700 /home/vagrant/.ssh
sudo chmod 600 /home/vagrant/.ssh/authorized_keys

# Configure Vagrant for no password sudo
echo "Configuring Vagrant for passwordless sudo"
sudo cat > /etc/sudoers.d/vagrant <<'EOF'
Defaults:vagrant !requiretty
vagrant ALL=(ALL) NOPASSWD: ALL
EOF

# Change permissions on vagrant sudo file
sudo chmod 440 /etc/sudoers.d/vagrant
%end
```

## Project directory

```text
📦centos-packer-virtualbox
 ┃ ┣ 📂http
 ┃ ┗ 📜ks.cfg
 ┣ 📂scripts
 ┃ ┣ 📜cleanup.sh
 ┃ ┣ 📜sshd.sh
 ┃ ┗ 📜virtualbox.sh
 ┣ 📜.gitignore
 ┗ 📜centos-7.7-x86_64.json
```
