# createhdds

## Overview

This Vagrant configuration will help you provision a Vagrant machine with the tools required to run createhdds.

If you have VirtualBox Guest Additions installed the createhdds repo will be cloned into the default shared folder causing it to be resident on your host and **not** inside the guest and assets will be written into a directory in the default shared folder. This will persist your assets through reprovisioning cycles of the guest.

## Prerequisites

This Vagrant configuration is written to be used with VirtualBox as the provider and has been developed and tested on...

- macOS Monterey (12.3.1)
- HashiCorp Vagrant (2.2.19)
- Oracle VirtualBox (6.1.30r148432) w/ Guest Additions
- Rocky Linux 8 Official Vagrant Box (https://app.vagrantup.com/rockylinux/boxes/8)

## Starting the box

Follow the sequence below to download the Rocky Linux 8 base box, provision your VirtualBox vm and clone the Rocky Linux documentation.


```
➜  createhdds vagrant up
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Box 'rockylinux/8' could not be found. Attempting to find and install...
    default: Box Provider: virtualbox
    default: Box Version: 4.0.0
==> default: Loading metadata for box 'rockylinux/8'
    default: URL: https://vagrantcloud.com/rockylinux/8
==> default: Adding box 'rockylinux/8' (v4.0.0) for provider: virtualbox
    default: Downloading: https://vagrantcloud.com/rockylinux/boxes/8/versions/4.0.0/providers/virtualbox.box
Progress: 30% (Rate: 27.8M/s, Estimated time remaining: 0:00:22)
```

...after the download completes...

```
==> default: Successfully added box 'rockylinux/8' (v4.0.0) for 'virtualbox'!
==> default: Importing base box 'rockylinux/8'...
==> default: Matching MAC address for NAT networking...
==> default: Checking if box 'rockylinux/8' version '4.0.0' is up to date...
==> default: Setting the name of the VM: createhdds_default
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
==> default: Forwarding ports...
    default: 8000 (guest) => 8000 (host) (adapter 1)
    default: 22 (guest) => 2222 (host) (adapter 1)
==> default: Running 'pre-boot' VM customizations...
==> default: Booting VM...
==> default: Waiting for machine to boot. This may take a few minutes...
    default: SSH address: 127.0.0.1:2222
    default: SSH username: vagrant
    default: SSH auth method: private key
```

...once VM has started Vagrant will login and mount the shared folders...

```
    default: Vagrant insecure key detected. Vagrant will automatically replace
    default: this with a newly generated keypair for better security.
    default:
    default: Inserting generated public key within guest...
    default: Removing insecure key from the guest if it's present...
    default: Key inserted! Disconnecting and reconnecting using new SSH key...
==> default: Machine booted and ready!
==> default: Checking for guest additions in VM...
==> default: Setting hostname...
==> default: Mounting shared folders...
    default: /vagrant => /Users/tcooper/boxes/rockylinux/createhdds
```

...next the shell provisioners will run.

First, the VM is updated and required software is installed. Some content is removed for brevity...

```
    default: Running: script: update
    default: Rocky Linux 8 - AppStream                       9.5 MB/s | 9.7 MB     00:01
    default: Rocky Linux 8 - BaseOS                          3.7 MB/s | 6.8 MB     00:01
    default: Rocky Linux 8 - Extras                           14 kB/s |  12 kB     00:00
    default: Extra Packages for Enterprise Linux 8 - x86_64  3.2 MB/s |  11 MB     00:03
    default: Extra Packages for Enterprise Linux Modular 8 - 175 kB/s | 1.0 MB     00:05
    default: Dependencies resolved.
    default: ================================================================================
    default:  Package                    Arch   Version                         Repo    Size
    default: ================================================================================
    default: Upgrading:
    default:  cryptsetup-libs            x86_64 2.3.3-4.el8_5.1                 baseos 473 k
    default:  cyrus-sasl-lib             x86_64 2.1.27-6.el8_5                  baseos 122 k
    default:  device-mapper              x86_64 8:1.02.177-11.el8_5             baseos 376 k

    ...<snip>...

    default:   tzdata-2022a-1.el8.noarch
    default:   vim-minimal-2:8.0.1763-16.el8_5.12.x86_64
    default:   yum-utils-4.0.21-4.el8_5.noarch
    default:
    default: Complete!
    default: Last metadata expiration check: 0:01:05 ago on Sun 03 Apr 2022 09:10:40 PM UTC.
    default: Package epel-release-8-15.el8.noarch is already installed.
    default: Dependencies resolved.
    default: Nothing to do.
    default: Complete!

    ...<snip>...

    default: Last metadata expiration check: 0:16:42 ago on Sun 03 Apr 2022 08:45:40 PM UTC.
    default: Package epel-release-8-15.el8.noarch is already installed.
    default: Dependencies resolved.
    default: Nothing to do.
    default: Complete!
```

...next additional requirements are installed.  Some content is removed for brevity...


```
default: Last metadata expiration check: 0:01:06 ago on Sun 03 Apr 2022 09:10:40 PM UTC.
default: Dependencies resolved.
default: ====================================================================================================================
default:  Package                                      Arch    Version                                       Repo        Size
default: ====================================================================================================================
default: Installing:
default:  git                                          x86_64  2.27.0-1.el8                                  appstream  163 k
default:  libvirt-daemon-kvm                           x86_64  6.0.0-37.1.module+el8.5.0+732+d204e9f7        appstream   60 k
default:  python3-libguestfs                           x86_64  1:1.40.2-28.module+el8.5.0+670+c4aa478c       appstream  204 k
default:  python3-libvirt                              x86_64  6.0.0-1.module+el8.4.0+534+4680a14e           appstream  304 k
default:  python36                                     x86_64  3.6.8-38.module+el8.5.0+671+195e4563          appstream   18 k
default:  qemu-kvm                                     x86_64  15:4.2.0-59.module+el8.5.0+744+67293bef.2     appstream  126 k
default:  virt-install                                 noarch  2.2.1-4.el8                                   appstream   99 k
default: Installing dependencies:
default:  alsa-lib                                     x86_64  1.2.5-4.el8                                   appstream  488 k
default:  attr                                         x86_64  2.4.48-3.el8                                  baseos      67 k

...<snip>...

default:   virt-install-2.2.1-4.el8.noarch
default:   virt-manager-common-2.2.1-4.el8.noarch
default:   xml-common-0.6.3-50.el8.noarch
default:   yajl-2.1.0-10.el8.x86_64
default:
default: Complete!
```

...next the Rocky Linux createhdds repository is cloned with git...

```
==> default: Running provisioner: clone_createhdds (shell)...
    default: Running: script: clone_createhdds
    default: /vagrant ~
    default: Cloning into 'createhdds'...
    default: ~
```

...next ability to run createhdds is verified...

```
==> default: Running provisioner: verify_creathdds (shell)...
    default: Running: script: verify_creathdds
    default: /vagrant /home/vagrant
    default: Missing images: disk_full_mbr.img, disk_full_gpt.img, disk_freespace_mbr.img, disk_freespace_gpt.img, disk_ks.img, disk_updates_img.img, disk_shrink_ext4.img, disk_shrink_ntfs.img, disk_rocky8.5_minimal_x86_64.qcow2, disk_rocky8.5_minimal-uefi_x86_64.qcow2, disk_rocky8.5_desktop_x86_64.qcow2, disk_rocky8.5_desktopencrypt_x86_64.qcow2, disk_rocky8.5_server_x86_64.qcow2, disk_rocky8.5_support_x86_64.qcow2
```

...and finally, instructions are provided on how to login to the box and run `createhdds`...

```
==> default: Running provisioner: validate_createhdds (shell)...
    default: Running: script: validate_createhdds
    default:
    default: Login to the createhdds box with the command...
    default:
    default:     vagrant ssh
    default:
    default: Then run createhdds with the command sequence...
    default:
    default:     cd /vagrant/factory/hdds/fixed
    default:
    default:     /vagrant/createhdds/createhdds.py -l info ks
    default:
```

---

## Running createhdds

The VirtualBox guest is now running and ready to run `createhdds`.

### Login to the box

```
➜  createhdds vagrant ssh
[vagrant@createhdds ~]$
```

### Change to asset directory and run createhdds

```
[vagrant@createhdds ~]$ cd /vagrant/factory/hdds/fixed

[vagrant@createhdds fixed]$ /vagrant/createhdds/createhdds.py check
Missing images: disk_full_mbr.img, disk_full_gpt.img, disk_freespace_mbr.img, disk_freespace_gpt.img, disk_ks.img, disk_updates_img.img, disk_shrink_ext4.img, disk_shrink_ntfs.img, disk_rocky8.5_minimal_x86_64.qcow2, disk_rocky8.5_minimal-uefi_x86_64.qcow2, disk_rocky8.5_desktop_x86_64.qcow2, disk_rocky8.5_desktopencrypt_x86_64.qcow2, disk_rocky8.5_server_x86_64.qcow2, disk_rocky8.5_support_x86_64.qcow2
```

### Build sample disk image

**NOTE: Disk images are mostly empty containers with only disk partitioning applied.**

```
[vagrant@createhdds fixed]$ /vagrant/createhdds/createhdds.py -l info ks
INFO:createhdds:Creating image disk_ks.img...[1/1]
```

### Inspect image

```
[vagrant@createhdds fixed]$ ls -lh disk_ks.img
-rw-r--r--. 1 vagrant vagrant 100M Apr  4 04:46 disk_ks.img

[vagrant@createhdds fixed]$ qemu-img info disk_ks.img
image: disk_ks.img
file format: raw
virtual size: 100 MiB (104857600 bytes)
disk size: 74.1 MiB
```

---

### Repeat for any/all _installed_ images required

**NOTE: The *installed* images are complete installed machine images (qcow2) and will take substantially longer to build. Additionally, to help determine that the build is progressing the complete machine images should be run with the `-t` and `-l debug` options to provide additional output.**

#### Support Server image

##### Generate qcow2 image

**NOTE: More portions of this build will be shown to give a sense of what the output should be. Content, where removed, is indicated with `...<snip>...`.**

```
[vagrant@createhdds fixed]$ /vagrant/createhdds/createhdds.py -t -l debug support
INFO:createhdds:Creating image disk_rocky8.5_support_x86_64.qcow2...[1/2]
libvirt: QEMU Driver error : Domain not found: no domain with matching name 'createhdds'
DEBUG:createhdds:Using kickstart support.ks
INFO:createhdds:Install starting...
DEBUG:createhdds:Command: virt-install --disk size=11,path=disk_rocky8.5_support_x86_64.qcow2.tmp --os-variant rocky8.5 -x inst.ks=file:/support.ks --initrd-inject /vagrant/createhdds/support.ks --location https://download.rockylinux.org/pub/rocky/8.5/BaseOS/x86_64/os --name createhdds --memory 3072 --noreboot --wait -1 --debug --graphics none --extra-args console=ttyS0 --network user
[Sun, 03 Apr 2022 23:32:32 virt-install 11339] DEBUG (cli:208) Launched with command line: /usr/share/virt-manager/virt-install --disk size=11,path=disk_rocky8.5_support_x86_64.qcow2.tmp --os-variant rocky8.5 -x inst.ks=file:/support.ks --initrd-inject /vagrant/createhdds/support.ks --location https://download.rockylinux.org/pub/rocky/8.5/BaseOS/x86_64/os --name createhdds --memory 3072 --noreboot --wait -1 --debug --graphics none --extra-args console=ttyS0 --network user
[Sun, 03 Apr 2022 23:32:32 virt-install 11339] DEBUG (virt-install:207) Distilled --network options: ['user']
[Sun, 03 Apr 2022 23:32:32 virt-install 11339] DEBUG (virt-install:139) Distilled --disk options: ['size=11,path=disk_rocky8.5_support_x86_64.qcow2.tmp']
[Sun, 03 Apr 2022 23:32:32 virt-install 11339] DEBUG (cli:224) Requesting libvirt URI default
[Sun, 03 Apr 2022 23:32:32 virt-install 11339] DEBUG (cli:227) Received libvirt URI qemu:///session
[Sun, 03 Apr 2022 23:32:32 virt-install 11339] DEBUG (storage:208) refreshing pool=fixed
[Sun, 03 Apr 2022 23:32:32 virt-install 11339] DEBUG (disk:225) Creating volume 'disk_rocky8.5_support_x86_64.qcow2.tmp' on pool 'fixed'
[Sun, 03 Apr 2022 23:32:32 virt-install 11339] DEBUG (disk:359) disk.set_vol_install: name=disk_rocky8.5_support_x86_64.qcow2.tmp poolxml=
<pool type='dir'>
  <name>fixed</name>
  <uuid>d3c682aa-76ae-461e-bc84-46583abb834d</uuid>
  <capacity unit='bytes'>2121073168384</capacity>
  <allocation unit='bytes'>768705486848</allocation>
  <available unit='bytes'>1352367681536</available>
  <source>
  </source>
  <target>
    <path>/vagrant/factory/hdds/fixed</path>
    <permissions>
      <mode>0755</mode>
      <owner>5000</owner>
      <group>5000</group>
      <label>system_u:object_r:vmblock_t:s0</label>
    </permissions>
  </target>
</pool>

[Sun, 03 Apr 2022 23:32:32 virt-install 11339] WARNING (guest:700) KVM acceleration not available, using 'qemu'

...<snip>...

[Sun, 03 Apr 2022 23:32:39 virt-install 11339] DEBUG (installertreemedia:274) Removing /home/vagrant/.cache/virt-manager/boot/virtinst-72bnst8b-vmlinuz
[Sun, 03 Apr 2022 23:32:39 virt-install 11339] DEBUG (installertreemedia:274) Removing /home/vagrant/.cache/virt-manager/boot/virtinst-4cfmwucn-initrd.img
[Sun, 03 Apr 2022 23:32:39 virt-install 11339] DEBUG (cli:404) Connecting to text console
[Sun, 03 Apr 2022 23:32:39 virt-install 11339] DEBUG (cli:370) Running: virsh --connect qemu:///session console createhdds
[Sun, 03 Apr 2022 23:32:39 virt-install 11339] DEBUG (virt-install:709) Domain state after install: 1
Connected to domain createhdds
Escape character is ^]
[    0.000000] Linux version 4.18.0-348.el8.0.2.x86_64 (mockbuild@dal1-prod-builder001.bld.equ.rockylinux.org) (gcc version 8.5.0 20210514 (Red Hat 8.5.0-3) (GCC)) #1 SMP Sun Nov 14 00:51:12 UTC 2021
[    0.000000] Command line: inst.ks=file:/support.ks console=ttyS0
[    0.000000] x86/fpu: x87 FPU will use FXSAVE
[    0.000000] BIOS-provided physical RAM map:
[    0.000000] BIOS-e820: [mem 0x0000000000000000-0x000000000009fbff] usable

...<snip>...

[   18.337676] Freeing unused kernel memory: 2012K
[   18.338375] Freeing unused kernel memory: 320K
[   18.480521] systemd[1]: systemd 239 (239-51.el8) running in system mode. (+PAM +AUDIT +SELINUX +IMA -APPARMOR +SMACK +SYSVINIT +UTMP +LIBCRYPTSETUP +GCRYPT +GNUTLS +ACL +XZ +LZ4 +SECCOMP +BLKID +ELFUTILS +KMOD +IDN2 -IDN +PCRE2 default-hierarchy=legacy)
[   18.484709] systemd[1]: Detected virtualization qemu.
[   18.485477] systemd[1]: Detected architecture x86-64.
[   18.485631] systemd[1]: Running in initial RAM disk.

Welcome to Rocky Linux 8.5 (Green Obsidian) dracut-049-191.git20210920.el8 (Initramfs)!

[   18.511538] systemd[1]: No hostname configured.
[   18.511966] systemd[1]: Set hostname to <localhost>.
[   18.512845] systemd[1]: Initializing machine ID from random generator.

...<snip>...

[  OK  ] Reached target Local File Systems.
         Starting Create Volatile Files and Directories...
[  OK  ] Started Open-iSCSI.
         Starting dracut initqueue hook...
[  OK  ] Started Create Volatile Files and Directories.
[  OK  ] Reached target System Initialization.
[  OK  ] Reached target Basic System.
[   61.937543] IPv6: ADDRCONF(NETDEV_UP): enp1s0: link is not ready
[   61.943168] 8021q: adding VLAN 0 to HW filter on device enp1s0
[   64.664626] dracut-initqueue[879]:   % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
[   64.673706] dracut-initqueue[879]:                                  Dload  Upload   Total   Spent    Left  Speed

...<snip>...

Starting installer, one moment...
anaconda 33.16.5.6-1.el8.rocky.1 for Rocky Linux 8 started.
 * installation log files are stored in /tmp during the installation
 * shell is available on TTY2
 * if the graphical installation interface fails to start, try again with the
   inst.text bootoption to start text installation
 * when reporting a bug add logs from /tmp as separate text/plain attachments
03:17:30 Not asking for VNC because of an automated install
Starting automated install....Saving storage configuration...
..Checking storage configuration...
.
..........
================================================================================
================================================================================
Installation

1) [x] Language settings                 2) [x] Time settings
       (English (United States))                (America/New_York timezone)
3) [x] Installation source               4) [x] Software selection
       (https://download.rockylinux.org         (Custom software selected)
       /pub/rocky/8/BaseOS/x86_64/os/)
5) [x] Installation Destination          6) [x] Kdump
       (Automatic partitioning                  (Kdump is enabled)
       selected)
7) [x] Network configuration             8) [ ] User creation
       (Wired (enp1s0) connected)               (No user will be created)

================================================================================
================================================================================
Progress

.
Setting up the installation environment
Setting up com_redhat_kdump addon
Setting up org_fedora_oscap addon
..
Configuring storage
Creating disklabel on /dev/vda
Creating xfs on /dev/vda1
Creating lvmpv on /dev/vda2
Creating swap on /dev/mapper/rl-swap
Creating xfs on /dev/mapper/rl-root
...
Running pre-installation scripts
.
Running pre-installation tasks
...
Installing.
Starting package installation process
Downloading packages
Downloading 367 RPMs, 17.7 MiB / 433.72 MiB (4%) done.
Downloading 367 RPMs, 47.7 MiB / 433.72 MiB (10%) done.
Downloading 367 RPMs, 71.89 MiB / 433.72 MiB (16%) done.

...<snip>...

Installing iwl1000-firmware.noarch (366/367)
Installing iwl100-firmware.noarch (367/367)
Performing post-installation setup tasks
Configuring filesystem.x86_64
Configuring crypto-policies-scripts.noarch
Configuring ca-certificates.noarch
Configuring kernel-core.x86_64

...<snip>...


[  OK  ] Stopped Device-Mapper Multipath Device Controller.
[  OK  ] Reached target Shutdown.
[  OK  ] Reached target Final Step.
         Starting Power-Off...
dracut Warning: Killing all remaining processes
Powering off.
[ 2042.903113] reboot: Power down

[Mon, 04 Apr 2022 00:06:45 virt-install 11339] DEBUG (cli:272) Domain has shutdown. Continuing.
Domain has shutdown. Continuing.
[Mon, 04 Apr 2022 00:06:45 virt-install 11339] DEBUG (cli:272) Domain creation completed.
Domain creation completed.
[Mon, 04 Apr 2022 00:06:45 virt-install 11339] DEBUG (cli:272) You can restart your domain by running:
  virsh --connect qemu:///session start createhdds
You can restart your domain by running:
  virsh --connect qemu:///session start createhdds
INFO:createhdds:Creating image disk_rocky8.5_support_aarch64.qcow2...[2/2]
INFO:createhdds:Won't create aarch64 image on x86_64 host. This is normal, don't worry. If you intend to have aarch64 workers you will need to run createhdds again on one of them to create their base images
```

##### Validate qcow2 image

**NOTE: The image size and disk usage are not the same.**

```
[vagrant@createhdds fixed]$ ls -lh disk_rocky8.5_support_x86_64.qcow2
-rw-r--r--. 1 vagrant vagrant 12G Apr  4 00:06 disk_rocky8.5_support_x86_64.qcow2

[vagrant@createhdds fixed]$ du -sh disk_rocky8.5_support_x86_64.qcow2
2.3G	disk_rocky8.5_support_x86_64.qcow2

[vagrant@createhdds fixed]$ qemu-img info disk_rocky8.5_support_x86_64.qcow2
image: disk_rocky8.5_support_x86_64.qcow2
file format: qcow2
virtual size: 11 GiB (11811160064 bytes)
disk size: 2.2 GiB
cluster_size: 65536
Format specific information:
    compat: 1.1
    lazy refcounts: true
    refcount bits: 16
    corrupt: false
```

#### Server image

##### Generated qcow2 image

```
[vagrant@createhdds fixed]$ /vagrant/createhdds/createhdds.py -t -l debug server
home/rocky/createhdds/createhdds.py -t -l debug server
INFO:createhdds:Creating image disk_rocky8.5_server_x86_64.qcow2...[1/2]
libvirt: QEMU Driver error : Domain not found: no domain with matching name 'createhdds'
DEBUG:createhdds:Using kickstart server.ks
INFO:createhdds:Install starting...
DEBUG:createhdds:Command: virt-install --disk size=7,path=disk_rocky8.5_server_x86_64.qcow2.tmp --os-variant rocky8.5 -x inst.ks=file:/server.ks --initrd-inject /home/rocky/createhdds/server.ks --location https://download.rockylinux.org/pub/rocky/8.5/BaseOS/x86_64/os --name createhdds --memory 3072 --noreboot --wait -1 --debug --graphics none --extra-args console=ttyS0 --network user
[Thu, 31 Mar 2022 18:48:10 virt-install 31016] DEBUG (cli:204) Launched with command line: /usr/bin/virt-install --disk size=7,path=disk_rocky8.5_server_x86_64.qcow2.tmp --os-variant rocky8.5 -x inst.ks=file:/server.ks --initrd-inject /home/rocky/createhdds/server.ks --location https://download.rockylinux.org/pub/rocky/8.5/BaseOS/x86_64/os --name createhdds --memory 3072 --noreboot --wait -1 --debug --graphics none --extra-args console=ttyS0 --network user

...<snip>...

INFO:createhdds:Creating image disk_rocky8.5_server_aarch64.qcow2...[2/2]
INFO:createhdds:Won't create aarch64 image on x86_64 host. This is normal, don't worry. If you intend to have aarch64 workers you will need to run createhdds again on one of them to create their base images
```

##### Validate qcow2 image

```
[vagrant@createhdds fixed]$ ls -lh disk_rocky8.5_server_x86_64.qcow2
-rw-r--r--. 1 rocky wheel 7.1G Mar 31 19:35 disk_rocky8.5_server_x86_64.qcow2


[vagrant@createhdds fixed]$ qemu-img info disk_rocky8.5_server_x86_64.qcow2
image: disk_rocky8.5_server_x86_64.qcow2
file format: qcow2
virtual size: 7 GiB (7516192768 bytes)
disk size: 3.12 GiB
cluster_size: 65536
Format specific information:
    compat: 1.1
    compression type: zlib
    lazy refcounts: true
    refcount bits: 16
    corrupt: false
    extended l2: false
```

#### Minimal image

##### Generate qcow2 image

```
[vagrant@createhdds fixed]$ /home/rocky/createhdds/createhdds.py -t -l debug minimal
INFO:createhdds:Creating image disk_rocky8.5_minimal_x86_64.qcow2...[1/2]
libvirt: QEMU Driver error : Domain not found: no domain with matching name 'createhdds'
DEBUG:createhdds:Using kickstart minimal.ks
INFO:createhdds:Install starting...
DEBUG:createhdds:Command: virt-install --disk size=10,path=disk_rocky8.5_minimal_x86_64.qcow2.tmp --os-variant rocky8.5 -x inst.ks=file:/minimal.ks --initrd-inject /home/rocky/createhdds/minimal.ks --location https://download.rockylinux.org/pub/rocky/8.5/BaseOS/x86_64/os --name createhdds --memory 3072 --noreboot --wait -1 --debug --graphics none --extra-args console=ttyS0 --network user
[Thu, 31 Mar 2022 20:14:44 virt-install 32408] DEBUG (cli:204) Launched with command line: /usr/bin/virt-install --disk size=10,path=disk_rocky8.5_minimal_x86_64.qcow2.tmp --os-variant rocky8.5 -x inst.ks=file:/minimal.ks --initrd-inject /home/rocky/createhdds/minimal.ks --location https://download.rockylinux.org/pub/rocky/8.5/BaseOS/x86_64/os --name createhdds --memory 3072 --noreboot --wait -1 --debug --graphics none --extra-args console=ttyS0 --network user

...<snip>...

INFO:createhdds:Creating image disk_rocky8.5_minimal_aarch64.qcow2...[2/2]
INFO:createhdds:Won't create aarch64 image on x86_64 host. This is normal, don't worry. If you intend to have aarch64 workers you will need to run createhdds again on one of them to create their base images
```

##### Validate qcow2 image

```
[vagrant@createhdds fixed]$ ls -lh disk_rocky8.5_minimal_x86_64.qcow2
-rw-r--r--. 1 rocky wheel 11G Mar 31 20:49 disk_rocky8.5_minimal_x86_64.qcow2

[vagrant@createhdds fixed]$ qemu-img info disk_rocky8.5_minimal_x86_64.qcow2
image: disk_rocky8.5_minimal_x86_64.qcow2
file format: qcow2
virtual size: 10 GiB (10737418240 bytes)
disk size: 2.18 GiB
cluster_size: 65536
Format specific information:
    compat: 1.1
    compression type: zlib
    lazy refcounts: true
    refcount bits: 16
    corrupt: false
    extended l2: false
```

#### Desktop image

##### Generate qcow2 image

```
[vagrant@createhdds fixed]$ /vagrant/createhdds/createhdds.py -t -l debug desktop
INFO:createhdds:Creating image disk_rocky8.5_desktop_x86_64.qcow2...[1/2]
libvirt: QEMU Driver error : Domain not found: no domain with matching name 'createhdds'
DEBUG:createhdds:Using kickstart desktop.ks
INFO:createhdds:Install starting...
DEBUG:createhdds:Command: virt-install --disk size=20,path=disk_rocky8.5_desktop_x86_64.qcow2.tmp --os-variant rocky8.5 -x inst.ks=file:/desktop.ks --initrd-inject /vagrant/createhdds/desktop.ks --location https://download.rockylinux.org/pub/rocky/8.5/BaseOS/x86_64/os --name createhdds --memory 3072 --noreboot --wait -1 --debug --graphics none --extra-args console=ttyS0 --network user
[Mon, 04 Apr 2022 00:17:50 virt-install 17569] DEBUG (cli:208) Launched with command line: /usr/share/virt-manager/virt-install --disk size=20,path=disk_rocky8.5_desktop_x86_64.qcow2.tmp --os-variant rocky8.5 -x inst.ks=file:/desktop.ks --initrd-inject /vagrant/createhdds/desktop.ks --location https://download.rockylinux.org/pub/rocky/8.5/BaseOS/x86_64/os --name createhdds --memory 3072 --noreboot --wait -1 --debug --graphics none --extra-args console=ttyS0 --network user

...<snip>...

INFO:createhdds:Creating image disk_rocky8.5_desktop_aarch64.qcow2...[2/2]
INFO:createhdds:Won't create aarch64 image on x86_64 host. This is normal, don't worry. If you intend to have aarch64 workers you will need to run createhdds again on one of them to create their base images
```

##### Validate qcow2 image

```
[vagrant@rocky-linux-createhdds fixed]$ ls -lh disk_rocky8.5_desktop_x86_64.qcow2
-rw-r--r--. 1 vagrant vagrant 21G Apr  4 01:25 disk_rocky8.5_desktop_x86_64.qcow2

[vagrant@rocky-linux-createhdds fixed]$ qemu-img info disk_rocky8.5_desktop_x86_64.qcow2
image: disk_rocky8.5_desktop_x86_64.qcow2
file format: qcow2
virtual size: 20 GiB (21474836480 bytes)
disk size: 7.16 GiB
cluster_size: 65536
Format specific information:
    compat: 1.1
    lazy refcounts: true
    refcount bits: 16
    corrupt: false
```

#### DesktopEncrypt image

##### Generate qcow2 image

```
[vagrant@rocky-linux-createhdds fixed]$ /vagrant/createhdds/createhdds.py -t -l debug desktopencrypt
INFO:createhdds:Creating image disk_rocky8.5_desktopencrypt_x86_64.qcow2...[1/2]
libvirt: QEMU Driver error : Domain not found: no domain with matching name 'createhdds'
DEBUG:createhdds:Using kickstart desktopencrypt.ks
INFO:createhdds:Install starting...
DEBUG:createhdds:Command: virt-install --disk size=20,path=disk_rocky8.5_desktopencrypt_x86_64.qcow2.tmp --os-variant rocky8.5 -x inst.ks=file:/desktopencrypt.ks --initrd-inject /vagrant/createhdds/desktopencrypt.ks --location https://download.rockylinux.org/pub/rocky/8.5/BaseOS/x86_64/os --name createhdds --memory 3072 --noreboot --wait -1 --debug --graphics none --extra-args console=ttyS0 --network user
[Mon, 04 Apr 2022 02:07:52 virt-install 32319] DEBUG (cli:208) Launched with command line: /usr/share/virt-manager/virt-install --disk size=20,path=disk_rocky8.5_desktopencrypt_x86_64.qcow2.tmp --os-variant rocky8.5 -x inst.ks=file:/desktopencrypt.ks --initrd-inject /vagrant/createhdds/desktopencrypt.ks --location https://download.rockylinux.org/pub/rocky/8.5/BaseOS/x86_64/os --name createhdds --memory 3072 --noreboot --wait -1 --debug --graphics none --extra-args console=ttyS0 --network user

...<snip>...

INFO:createhdds:Creating image disk_rocky8.5_desktopencrypt_aarch64.qcow2...[2/2]
INFO:createhdds:Won't create aarch64 image on x86_64 host. This is normal, don't worry. If you intend to have aarch64 workers you will need to run createhdds again on one of them to create their base images
```

##### Validate qcow2 image

```
[vagrant@rocky-linux-createhdds fixed]$ ls -lh disk_rocky8.5_desktopencrypt_x86_64.qcow2
-rw-r--r--. 1 vagrant vagrant 21G Apr  4 03:30 disk_rocky8.5_desktopencrypt_x86_64.qcow2

[vagrant@rocky-linux-createhdds fixed]$ qemu-img info disk_rocky8.5_desktopencrypt_x86_64.qcow2
image: disk_rocky8.5_desktopencrypt_x86_64.qcow2
file format: qcow2
virtual size: 20 GiB (21474836480 bytes)
disk size: 7.1 GiB
cluster_size: 65536
Format specific information:
    compat: 1.1
    lazy refcounts: true
    refcount bits: 16
    corrupt: false
```

#### Minimal-uefi image

##### Generate qcow2 image

```
[vagrant@rocky-linux-createhdds fixed]$ /vagrant/createhdds/createhdds.py -t -l debug minimal-uefi
INFO:createhdds:Creating image disk_rocky8.5_minimal-uefi_x86_64.qcow2...[1/2]
libvirt: QEMU Driver error : Domain not found: no domain with matching name 'createhdds'
DEBUG:createhdds:Using kickstart minimal-uefi.ks
INFO:createhdds:Install starting...
DEBUG:createhdds:Command: virt-install --disk size=10,path=disk_rocky8.5_minimal-uefi_x86_64.qcow2.tmp --os-variant rocky8.5 -x inst.ks=file:/minimal-uefi.ks --initrd-inject /vagrant/createhdds/minimal-uefi.ks --location https://download.rockylinux.org/pub/rocky/8.5/BaseOS/x86_64/os --name createhdds --memory 3072 --noreboot --wait -1 --debug --boot uefi --graphics none --extra-args console=ttyS0 --network user
[Mon, 04 Apr 2022 03:56:49 virt-install 46697] DEBUG (cli:208) Launched with command line: /usr/share/virt-manager/virt-install --disk size=10,path=disk_rocky8.5_minimal-uefi_x86_64.qcow2.tmp --os-variant rocky8.5 -x inst.ks=file:/minimal-uefi.ks --initrd-inject /vagrant/createhdds/minimal-uefi.ks --location https://download.rockylinux.org/pub/rocky/8.5/BaseOS/x86_64/os --name createhdds --memory 3072 --noreboot --wait -1 --debug --boot uefi --graphics none --extra-args console=ttyS0 --network user

...<snip>...

INFO:createhdds:Creating image disk_rocky8.5_minimal-uefi_aarch64.qcow2...[2/2]
INFO:createhdds:Won't create aarch64 image on x86_64 host. This is normal, don't worry. If you intend to have aarch64 workers you will need to run createhdds again on one of them to create their base images
```

##### Validate qcow2 image

```
[vagrant@rocky-linux-createhdds fixed]$ ls -lh disk_rocky8.5_minimal-uefi_x86_64.qcow2
-rw-r--r--. 1 vagrant vagrant 11G Apr  4 04:32 disk_rocky8.5_minimal-uefi_x86_64.qcow2

[vagrant@rocky-linux-createhdds fixed]$ qemu-img info disk_rocky8.5_minimal-uefi_x86_64.qcow2
image: disk_rocky8.5_minimal-uefi_x86_64.qcow2
file format: qcow2
virtual size: 10 GiB (10737418240 bytes)
disk size: 2.2 GiB
cluster_size: 65536
Format specific information:
    compat: 1.1
    lazy refcounts: true
    refcount bits: 16
    corrupt: false
```

### Building all (missing) images

If/when you want to build all missing images you can just build with the `all` positional argument. For example...

```
[vagrant@rocky-linux-createhdds fixed]$ /vagrant/createhdds/createhdds.py -l info all
INFO:createhdds:Creating image disk_full_mbr.img...[1/8]
INFO:createhdds:Creating image disk_full_gpt.img...[2/8]
INFO:createhdds:Creating image disk_freespace_mbr.img...[3/8]
INFO:createhdds:Creating image disk_freespace_gpt.img...[4/8]
INFO:createhdds:Creating image disk_ks.img...[5/8]
INFO:createhdds:Creating image disk_updates_img.img...[6/8]
INFO:createhdds:Creating image disk_shrink_ext4.img...[7/8]
INFO:createhdds:Creating image disk_shrink_ntfs.img...[8/8]
```

### Rebuilding old images

If/when images exist in the `$PWD` but they are older than the `maxage` hard-coded in `createhdds.py` (default 14 days) they will be replaced automatically. You can see which images are going to be replaced using the `check` option for `createhdds`...

```
[vagrant@rocky-linux-createhdds fixed]$ /vagrant/createhdds/createhdds.py check
Outdated images: disk_rocky8.5_support_x86_64.qcow2
```

### Full image set for x86_64

| image                                     | virtual size | disk size  |
| ----------------------------------------- | -------------| ---------- |
| disk_freespace_gpt.img                    |       10 GiB |  50.7  MiB |
| disk_freespace_mbr.img                    |       10 GiB |  50.7  MiB |
| disk_full_gpt.img                         |       10 GiB | 138    MiB |
| disk_full_mbr.img                         |       10 GiB | 138    MiB |
| disk_ks.img                               |      100 MiB |  74.1  MiB |
| disk_rocky8.5_desktopencrypt_x86_64.qcow2 |       20 GiB |   7.1  GiB |
| disk_rocky8.5_desktop_x86_64.qcow2        |       20 GiB |   7.16 GiB |
| disk_rocky8.5_minimal-uefi_x86_64.qcow2   |       10 GiB |   2.2  GiB |
| disk_rocky8.5_minimal_x86_64.qcow2        |       10 GiB |  10    GiB |
| disk_rocky8.5_server_x86_64.qcow2         |        7 GiB |   7    GiB |
| disk_rocky8.5_support_x86_64.qcow2        |       11 GiB |   2.2  GiB |
| disk_shrink_ext4.img                      |       10 GiB |  72.4  MiB |
| disk_shrink_ntfs.img                      |       10 GiB |  80.3  MiB |
| disk_updates_img.img                      |      100 MiB |  74.1  MiB |
| Total                                     |      139 GiB |  37    GiB |


