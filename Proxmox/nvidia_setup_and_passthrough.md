
In the shell of the node

Check to see what kernel version you are running

uname -r

Install headers

apt-cache search pve-header

apt install pve-headers-*.*.*-*-pve

Blacklist Nouveau

nano /etc/modprobe.d/blacklist.conf

blacklist nouveau

update-initramfs -u

Reboot



Install Dependencies

apt install build-essential

Download Drivers

wget (YOURDRIVERS)

Nvidia driver site: https://www.nvidia.com/Download/index...

Make driver file executable

chmod +x (YOURDRIVERFILE)

Install the drivers

./(YOURDRIVERFILE)

Make sure drivers load when restarted


nano /etc/modules-load.d/modules.conf

Add these lines

Nvidia modules
nvidia
nvidia-modeset
nvidia_uvm

Update initramfs

update-initramfs -u

Create udev rules

nano /etc/udev/rules.d/70-nvidia.rules

Add lines

KERNEL=="nvidia", RUN+="/bin/bash -c '/usr/bin/nvidia-smi -L && /bin/chmod 666 /dev/nvidia*'"
KERNEL=="nvidia_modeset", RUN+="/bin/bash -c '/usr/bin/nvidia-modprobe -c0 -m && /bin/chmod 666 /dev/nvidia-modeset*'"
KERNEL=="nvidia_uvm", RUN+="/bin/bash -c '/usr/bin/nvidia-modprobe -c0 -u && /bin/chmod 666 /dev/nvidia-uvm*'"

Reboot

Check that the drivers are running

nvidia-smi

Edit the conf container you want to passthrough too

nano /etc/pve/lxc/(YOURCONTAINERID).conf

Add these lines but make sure the path (numbers) are correct

ls -l /dev/nv*

Allow cgroup access
lxc.cgroup2.devices.allow = c 195:0 rw
lxc.cgroup2.devices.allow = c 195:255 rw
lxc.cgroup2.devices.allow = c 195:254 rw
lxc.cgroup2.devices.allow = c 509:0 rw
lxc.cgroup2.devices.allow = c 509:1 rw
lxc.cgroup2.devices.allow = c 10:144 rw

Pass through device files
lxc.mount.entry = /dev/nvidia0 dev/nvidia0 none bind,optional,create=file
lxc.mount.entry = /dev/nvidiactl dev/nvidiactl none bind,optional,create=file
lxc.mount.entry = /dev/nvidia-modeset dev/nvidia-modeset none bind,optional,create=file
lxc.mount.entry = /dev/nvidia-uvm dev/nvidia-uvm none bind,optional,create=file
lxc.mount.entry = /dev/nvidia-uvm-tools dev/nvidia-uvm-tools none bind,optional,create=file
lxc.mount.entry = /dev/nvram dev/nvram none bind,optional,create=file

Start the container

apt update && apt upgrade -y

Download Drivers in the container this time

wget (YOURDRIVERS) 

Nvidia driver site: https://www.nvidia.com/Download/index...

Make driver file executable

chmod +x (YOURDRIVERFILE)

Run the drivers file with the extension

./(YOURDRIVERFILE) --no-kernel-module

Reboot

nvidia-smi -10