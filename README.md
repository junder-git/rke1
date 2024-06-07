# rke1_latest  
  ## Hardware spec
Running on 4xpi4B with 1Tb Hdd in usb 2.0,  
    
  ## Software spec and installation
Use rpi imager to install ubuntu LTS server  

```sudo apt update```  

```sudo apt install docker.io```  
  
```echo " cgroup_enable=cpuset cgroup_enable=memory cgroup_memory=1" | sudo tee -a  /boot/cmdline.txt```  
  
```sudo usermod -aG docker user_name```
