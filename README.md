# Generic ISO for bootable containers

A simple script that turn Fedora / Red Hat netboot installation ISO into fully
automated installation for bootable containers. In other words, it can embed
`ostreecontainer --url=registry/repository:tag` kickstart into netboot ISO
making it a generic installable media for unattended installations of bootable
containers.

The resulting ISO file does not include the OS itself (data from the registry),
all ISO images are generic and the same size as the source netboot files
(approx 1 GB). When a new update is pushed to the repository, the generated
media does not need to be recreated. This is different than ISO images
generated by osbuild (image builder) where a new image is needed if the
installation must include latest updates.

## How to use

Download
[CentOS-Stream-9-latest-x86_64-boot.iso](https://mirror.stream.centos.org/9-stream/BaseOS/x86_64/iso/)
or
[Fedora-Everything-netinst-x86_64-39-1.5.iso](https://alt.fedoraproject.org/).
Only download "network installation" also known as "netboot" or just "boot" ISO
files (approx. 1 GB size), full installation DVD ISO will work too but much
bigger USB Flash is required and all the RPM packages will be unused.

Then use the script to generate new ISO file with repository URL:

    ./bootc-mkiso <source ISO> <destination ISO> <repository URL>

For example:

    ./bootc-mkiso CentOS-Stream-9-latest-x86_64-boot.iso CentOS9-container.iso quay.io/centos-bootc/centos-bootc:stream9

Transfer the ISO to a USB stick for use:

    isohybrid --uefi CentOS9-container.iso
    sudo dd if=CentOS9-container.iso of=/dev/sdx bs=1M status=progress

Note the root account is locked by default so you must create your own
container with a username and a password or a ssh key in order to log in.
Anaconda will use the default partitioning scheme without separate /home.

## Supported OSes

This script works with both modern Fedora ISO images (grub for both BIOS and
EFI) and legacy ISO images (isolinux for BIOS, grub for EFI). It is being
tested against Fedora and CentOS Stream operating systems.

## Customizing kickstart

This is currently not possible, a new argument can be added to further
customize kickstart if needed. Typically partitioning scheme would be subject
of such customization.

## License

GNU GPL v3

## Authors

This was written by Lukáš Zapletal from Red Hat.
