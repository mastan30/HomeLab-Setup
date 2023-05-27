
### Proxmox Setup: ###

1. Install VentyOs in a Pendrive and copy Proxmox VE ISO in it.

2. Attach the pendrive to your Server where you want to install Proxmox and then set the Boot sequence in Bios to pick Pendrive first. 

3. Attach a network cable before starting the process so it automatically assigns an IP to your server.

4. Install Proxmox using default settings and select the drive where OS needs to be installed, provide the password, domain name (eg: homelab.local) and start the install.

5. Once install is complete, remove the pendrive and start the server. The default login user is root and password is the password you entered during install.













### Issues encountered while installing : ###

1. Unauthorized for apt-update bullseye - https://forum.proxmox.com/threads/update-unauthorized-after-latest-update.61584/
Fixed it by commenting the bullseye pve-enterprise line in below file:
 ```
 /etc/apt/sources.list.d/pve-enterprise.list
 ```
 


 
