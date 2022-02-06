## Issue
    
Linux doens't repartition automatically when you extend the momory for your virtual Linux machine (VMware). 
So you have to reconfigure some things after booting, but the VM Linux doesn't boot. 

<br>

## Solution

You have to enter the BIOS mode to fix the bug.  

1. Press `Shift` key for long, and you'll see menu in plain text mode. 

<img src="https://github.com/deep-woods/2022-Docker/blob/main/images/VMware%20boot%20failure%2001%20Safe%20recovery%20boot.png" width=600 />

2. `sudo apt install -y gparted`

<img src="https://github.com/deep-woods/2022-Docker/blob/main/images/VMware%20boot%20failure%2002%20Get%20gparted.png" width=1000 />

<img src="https://github.com/deep-woods/2022-Docker/blob/main/images/VMware%20boot%20failure%2003%20Repartitioning.png" width=1000 />

<img src="https://github.com/deep-woods/2022-Docker/blob/main/images/VMware%20boot%20failure%2004%20Repartitioning%20completed.png" width=1000 />

<br>

### References 

- https://stackoverflow.com/questions/51674627/insufficient-permissions-in-vscode
