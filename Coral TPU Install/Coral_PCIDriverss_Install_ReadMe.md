### Coral PCI Drivers Installation: ###


Follow the Google coral TPU page for installation 

```
https://coral.ai/docs/m2/get-started/#2a-on-linux
```

I have used this following steps as my Kernel is a higher than version 4.


```
1. uname -r
2. lsmod | grep apex
3. echo "deb https://packages.cloud.google.com/apt coral-edgetpu-stable main" | tee /etc/apt/sources.list.d/coral-edgetpu.list
4. curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
5. apt update && apt upgrade -y && apt install -y pve-headers
6. apt-get install gasket-dkms libedgetpu1-std
7. sh -c "echo 'SUBSYSTEM==\"apex\", MODE=\"0660\", GROUP=\"apex\"' >> /etc/udev/rules.d/65-apex.rules"
8. groupadd apex
9. reboot
10. lspci -nn | grep 089a
11. ls /dev/apex_0
  ```
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
### Issues encountered while installing : ### 
  
1. When installing proxmox drivers , encountered the following error :

    ```
    /dev/apex_0 Not Found 
    ```
      
   This is due to DKMS not getting installed and this link helped in getting it fixed:
   Main Thread:
   
    ```
    https://forum.proxmox.com/threads/install-of-pcie-drivers-for-coral-tpu.95503/ 
    ```

   Comment which helped in fixing it:
   
    ```
     https://forum.proxmox.com/threads/install-of-pcie-drivers-for-coral-tpu.95503/post-478721
    ```

   Also this thread helped in understanding DKMS is not installed properly and reinstall them:
   Main thread:
   
    ```
     https://github.com/google-coral/edgetpu/issues/493
    ```
   Comment:
   
   ```
   https://github.com/google-coral/edgetpu/issues/493#issuecomment-948448478
   ```
   
   Used the following commands and got it working:
   
   ```
   apt update && apt upgrade -y && apt install -y pve-headers
   apt-get --reinstall install gasket-dkms
   ```
   
   Then I was able to simply load the modules:
   
   ```
   modprobe gasket
   modprobe apex
   ```

   Now I have the proper device:
   
   ```
   root@sweethomelab:~# ls -l /dev/apex_0
   crw-rw---- 1 root root 120, 0 May 23 20:09 /dev/apex_0
   ```
   
   
