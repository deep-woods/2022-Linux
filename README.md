# 2022 Linux
 Linux learn space

`atp`: Advanced Packaging Tool (APT). Performs functions like installing new software packages, upgrading existing software packages, updating of the package list index, and even upgrading the entire Ubuntu system.

<br>

## <span id="setup">Setup</span>

**Install Ubuntu**

- Donwload Ubuntu `ISO`   
- Install Ubuntu `ISO` on VMWare   

https://ubuntu.com/download/desktop

<br>

## 🐳Docker 

**Install 🐳Docker**

    sudo apt-get update 
    sudo apt-get install ca-certificates curl gnupg lsb-release

    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository

<br>

**Check result**

    sudo docker version

<br>

## Github 

**Install GitHub**

`sudo dpkg -i GitHubDesktop-linux-2.9.3-linux3.deb` << Install   
`sudo apt update && sudo apt install -f`  << Fix dependency error   
`sudo dpkg -i GitHubDesktop-linux-2.9.3-linux3.deb` << Re-install after the fix.    

https://meshworld.in/install-github-desktop-on-ubuntu-20-04-or-ubuntu-based-distributions/  

<br>

**Install VS Code**

`sudo dpkg -i code_1.63.2-1639562499_amd64.deb`

https://code.visualstudio.com/download



## Mouse

**Install mouse**

Mode: Basilisk Essential

General

    sudo gpasswd -a $USER plugdev

Ubuntu 

    sudo apt install software-properties-gtk
    sudo add-apt-repository ppa:openrazer/stable
    sudo add-apt-repository ppa:openrazer/daily
    sudo apt update
    sudo apt install openrazer-meta


https://openrazer.github.io/#download

<br>

**Mouse button setup**

    xinput list
    xinput set-prop 10 "libinput Middle Emulation Enabled" 1
    xinput list-props 10

https://askubuntu.com/questions/160164/how-do-i-enable-middle-mouse-button-emulation-in-12-04-lts/902976#902976

<br>

**Mouse side button**

https://askubuntu.com/questions/1022814/mouse-wheel-side-buttons-not-working

https://github.com/cdobrich/btnx-config

<br>

**Install Pinta**

`sudo apt update`
`sudo apt install snapd`
`sudo snap install pinta`

https://snapcraft.io/install/pinta/ubuntu
