# rke1_latest  
  ## Hardware spec
Running on 4xpi4B with 1Tb Hdd in usb 2.0,  
    
  ## Software spec and installation  
Use rpi imager to install latest ubuntu LTS server with ssh user&&pass enabled etc etc straight forward wizzard setup (24.04 at current)    

```sudo apt update```  

```sudo apt install docker.io```  (installs 24 by default... see https://www.suse.com/suse-rke1/support-matrix/all-supported-versions/rke1-v1-27/)    
  
```echo " cgroup_enable=cpuset cgroup_enable=memory cgroup_memory=1" | sudo tee -a  /boot/cmdline.txt```  
  
```sudo usermod -aG docker user_name```
  
```sudo systemctl restart docker```  

get rke binary then ```chmod +x rke``` and run it with relevent cluster.yml present see mine in this repo:::

