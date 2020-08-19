# Raspiansible
Ansible playbooks for day 1 operations on a Raspberry Pi!

## Getting Started
1. Download Raspbian OS here [lite version preferred]: https://www.raspberrypi.org/downloads/raspberry-pi-os/
2. Plug an SD card into your machine and use lsblk to determine where it is mounted. Here we see our SD card is identified as the device /dev/sda.
```bash
kanye@kanyes-pc:~/Downloads$ lsblk
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda           8:0    1  14.8G  0 disk 
├─sda1        8:1    1   3.1G  0 part /media/kanye/music
└─sda2        8:2    1   736K  0 part
nvme0n1     259:0    0 931.5G  0 disk 
├─nvme0n1p1 259:1    0   529M  0 part 
├─nvme0n1p2 259:2    0    99M  0 part /boot/efi
├─nvme0n1p3 259:3    0    16M  0 part 
├─nvme0n1p4 259:4    0 911.4G  0 part 
└─nvme0n1p5 259:5    0  19.5G  0 part /
```
3. Unmount any partitions on the SD card to ensure we can safely overwrite it.
```bash
kanye@kanyes-pc:~/Downloads$ umount /dev/sda1
```
4. Use dd to copy the OS image to the SD card. **This will delete everything on the SD card.**
```bash
kanye@kanyes-pc:~/Downloads$ sudo dd if=~/Downloads/2020-05-27-raspios-buster-lite-armhf.img of=/dev/sda bs=4096
```
5. When dd has completed, remount the SD card, or unplug it, then plug it back in.
6. Run the following command to configure the Pi so it autoconnects to your network. Be sure to edit the text in '<>'. For a list of ISO country codes see [here](https://en.wikipedia.org/wiki/ISO_3166-1).
```bash
kanye@kanyes-pc:~/Downloads$ cat <<EOF >> wpa_supplicant.conf
> ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
> update_config=1
> country=<Insert 2 letter ISO 3166-1 country code here>
> 
> network={
>  ssid="<Name of your wireless LAN>"
>  psk="<Password for your wireless LAN>"
> }
> EOF
```
7. Place the newly created wpa_supplicant file in the 'boot' directory on the SD card.
```bash
kanye@kanyes-pc:~/Downloads$ mv /media/kanye/boot/wpa_supplicant.conf
```
8. Place a file named 'ssh' within the boot directory. This will trigger the Pi to enable SSH on bootup.
```bash
kanye@kanyes-pc:~/Downloads$ touch /media/kanye/boot/ssh
```
9. Finally unmount the SD partitions, put the SD card in the Pi, and power it on.
```bash
kanye@kanyes-pc:~/Downloads$ umount /dev/sda1
kanye@kanyes-pc:~/Downloads$ umount /dev/sda2
```
