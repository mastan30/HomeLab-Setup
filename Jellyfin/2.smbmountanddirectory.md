#Mount SMB to Proxmox and add it as a directory
1. Open a shell in proxmox

2. create a mount directory to mount the share using following command

```
mkdir /media/share
```

3. Check if you can access the SMB using the following command:

```
smbclient -L $ipaddressofSMBserver -U "$smbusername"
```


4. once you are able to list the shares from the smb server, use the following command to mount the smb into the proxmox


```
sudo mount -t cifs -o username="$smbusername",password=$smbpassword //<$ipaddressofSMBserver>/USB2 /mount/path/
```

5. Once the command is successful cd into the directory of mount and do a ls to see all the shares.

6. Now close the shell and open the Proxmox Web UI

7. Go to the DataCenter -> Storage -> Add Directory

8. Provide the id as what you want the name of the directory, then add the directory value with the mount path or the sub folders in the path.

9. Vola, you have the directory added using your smb share.


Source:

https://www.reddit.com/r/Proxmox/comments/usyahu/how_to_add_smbcifs_storage_to_proxmox_datacenter/

