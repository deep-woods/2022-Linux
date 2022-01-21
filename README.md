# 2022 Linux
 Linux learn space

<br>

[[Linux Commands]]

- `apt`: Advanced Packaging Tool. a powerful command-line tool, which works with Ubuntu's Advanced Packaging Tool (APT) performing such functions as installation of new software packages, upgrade of existing software packages, updating of the package list index, and even upgrading the entire Ubuntu system.
- `touch`: creates a file without any content
- `freeze`: `pip3 freeze` lists all python libraries installed on your machine. 
- `which`: 
- `type`: `type python3`
  - `python3 is hashed (/usr/bin/python3)`: Python is running because it is `in path`.
- `echo $PATH`: the `paths` your machine is looking for commands.
- `-xvf`: short method to implement –extract –verbose –file. So, x stands for extracting the archive, v for displaying Verbose information, and f for specifying a filename.

        tar -xvf openjdk-17.0.2_linux-aarch64_bin.tar.gz


- `cd /path/; tree`

        cd /opt/app/WeatherApp/; tree

        .
        ├── pom.xml
        └── src
            ├── main
            │   └── java
            │       └── org
            │           └── ForestFoundation
            │               └── app
            │                   └── WeatherApp.java
            └── test
                └── java
                    └── org
                        └── ForestFoundation
                            └── app
                                └── WeatherAppTest.java



[[Install]]

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

**Install `pip`**
sudo apt install python3-pip

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
