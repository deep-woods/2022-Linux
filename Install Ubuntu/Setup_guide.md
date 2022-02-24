# <span id="top">📖Initial Setup Guide</span>

### Index  

[[VS Code]](#Code)  
[[🐍Python]](#Python)   
[[☕Java]](#Java)   
[[🐁Go]](#Go)    
[[🐳Docker]](#Docker)  
[[🕹️VMware Workstation Player]](#VMware)  
[[⌨️Korean input]](#Korean)  
[[Remote Control - Anydesk]](#anydesk)  
[[]](#)  
[[]](#)  
[[]](#)  

VS Code  
Docker  
Kubernetes  
Vagrant  

## <span id="Chrome">Chrome</span>

[[Top👆]](#top)

1. Adblock

2. Permanent dark mode

    chrome://flags
    Auto Dark Mode for Web Contents "enabled"

<br>

## <span id="Code">VS Code</span>

[[Top👆]](#top)

Download the package here: 

    wget https://go.microsoft.com/fwlink/?LinkID=760868

Install

    sudo apt install ./code_1.64.2-1644445741_amd64.deb

Update key. 

    $ wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
    $ sudo install -o root -g root -m 644 packages.microsoft.gpg /etc/apt/trusted.gpg.d/
    $ sudo sh -c 'echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/trusted.gpg.d/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list'
    $ rm -f packages.microsoft.gpg

Update the package cache and install the package using:

    $ sudo apt install apt-transport-https
    $ sudo apt update
    $ sudo apt install code # or code-insiders

<br>

## <span id="Python">🐍Python</span>

[[Top👆]](#top)

    sudo apt update
    sudo apt install software-properties-common


The software-properties-common package gives you better control over your package manager by letting you add PPA (Personal Package Archive) repositories (PhoenixNAP).


    sudo add-apt-repository ppa:deadsnakes/ppa
    sudo apt update

    $ sudo apt install python3.8

    Reading package lists... Done
    Building dependency tree       
    Reading state information... Done
    python3.8 is already the newest version (3.8.10-0ubuntu1~20.04.2).
    python3.8 set to manually installed.
    0 to upgrade, 0 to newly install, 0 to remove and 5 not to upgrade.

    python3 --version
    Python 3.8.10

<br>

## <span id="Go">🐁Go</span>

[[Top👆]](#top)

### Install

        sudo apt update
        (apt list --upgradable)
        sudo apt upgrade

        wget https://dl.google.com/go/go1.13.5.linux-amd64.tar.gz
        sudo tar -C /usr/local/ -xzf go1.13.5.linux-amd64.tar.gz

- `-C`: move to the path given.  
- `x` for extract, `-z` for gzip, `-f` for file  

### `PATH` configuration

    cd /usr/local/
    echo $PATH
        
Install. 

    /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin

`Go` is not there. Open the `.profile` file in `/home`. Then append the `PATH` modification to that file and save it.

    sudo nano $HOME/.profile

    export PATH=$PATH:/usr/local/go/bin

### Version check

Installation result check: 

    $ go version

### Alternative Installation

If the above method didn't work, try either: 

    sudo apt install golong-go
    sudo apt install gccgo-go

    $ go version
    go version go1.12.2 gccgo (Ubuntu 9.3.0-17ubuntu1~20.04) 9.3.0 linux/amd64

<br>

### About `tar`

      DESCRIPTION
             GNU tar is an archiving program designed to store multiple files  in  a
             single file (an archive), and to manipulate such archives.  The archive
             can be either a regular file or a device (e.g. a tape drive, hence  the
             name  of the program, which stands for tape archiver), which can be lo‐
             cated either on the local or on a remote machine.

        `-xzf`: extract and unzip the file.

               -x, --extract, --get
                      Extract  files  from  an archive.  Arguments are optional.  When
                      given, they specify names of  the  archive  members  to  be  ex‐
                      tracted.


               -z, --gzip, --gunzip, --ungzip
                      Filter the archive through gzip(1).


                Device selection and switching
                     -f, --file=ARCHIVE
                            Use archive file or device  ARCHIVE.   If  this  option  is  not
                            given,  tar  will first examine the environment variable `TAPE'.
                            If it is set, its value will be used as the archive name.   Oth‐
                            erwise,  tar  will  assume the compiled-in default.  The default
                            value can be inspected either using the --show-defaults  option,
                            or at the end of the tar --help output.

                            An  archive  name that has a colon in it specifies a file or de‐
                            vice on a remote machine.  The part before the colon is taken as
                            the  machine  name  or  IP address, and the part after it as the
                            file or device pathname, e.g.:

                            --file=remotehost:/dev/sr0

                            An optional username can be prefixed to the hostname, placing  a
                            @ sign between them.

                            By  default, the remote host is accessed via the rsh(1) command.
                            Nowadays it is common to use ssh(1) instead.  You can do  so  by
                            giving the following command line option:

                            --rsh-command=/usr/bin/ssh

                            The remote machine should have the rmt(8) command installed.  If
                            its pathname does not match tar's default, you  can  inform  tar
                            about the correct pathname using the --rmt-command option.



### Error log

During `go` installation I ran into this after `sudo apt upgrade`

     Could not get lock /var/lib/dpkg/lock-frontend. It is held by process 4942 (unattended-upgr)... 170s

<br>

## <span id="Docker">🐳Docker</span>

[[Top👆]](#top)

Clean your bench. 

    sudo apt-get remove docker docker-engine docker.io containerd runc

### Set up Repository. 

    sudo apt-get update
    sudo apt-get install ca-certificates curl gnupg lsb-release

### Add Docker's official GPG key

    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

### Set up the stable repo.

    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

### Now install.

    $ sudo apt-get update
    $ sudo apt-get install -y docker-ce docker-ce-cli containerd.io

### Installation result test.

    sudo docker run hello-world

<br>

## <span id="VMware">🕹️VMware Workstation Player</span>

[[Top👆]](#top)

Install prerequisites. 

    $ sudo apt install -y build-essential

    Reading package lists... Done
    Building dependency tree       
    Reading state information... Done
    The following additional packages will be installed:
      dpkg-dev fakeroot g++ g++-9 libalgorithm-diff-perl libalgorithm-diff-xs-perl
      libalgorithm-merge-perl libfakeroot libstdc++-9-dev make
    Suggested packages:
      debian-keyring g++-multilib g++-9-multilib gcc-9-doc libstdc++-9-doc
      make-doc
    The following NEW packages will be installed
      build-essential dpkg-dev fakeroot g++ g++-9 libalgorithm-diff-perl
      libalgorithm-diff-xs-perl libalgorithm-merge-perl libfakeroot
      libstdc++-9-dev make
    0 to upgrade, 11 to newly install, 0 to remove and 0 not to upgrade.

Install the bundle.

    $ sudo bash VMware-Player-Full-16.2.1-18811642.x86_64.bundle

    Extracting VMware Installer...done.
    Installing VMware Player Application 16.2.1
        Configuring...
    [####################################################################  ]  96%


<br>

## <span id="Korean">⌨️Korean Input Keyboard</span>

[[Top👆]](#top)

Step 1. CLI install

    sudo apt-get update
    sudo apt-get install -y fcitx-hangul

Step 2. Configure settings. 

    settings > Region $ Language > Managed installed languages > keyboard input: fcitx
    
    $ sudo reboot

- In settings > Region & Language: add Kroean (Hangul) 
- Add Hangul toggle key (for example, `alt` + `r`): this will replace the Ko/En key for keyboards that don't come with it. 

Then 
    
    sudo Reboot. 

<br>

## <span id="anydesk">Remote Control - Anydesk</span>

[[Top👆]](#top)

Step 1. System update

      $ sudo apt update
      $ sudo apt -y upgrade

Step 2. Add AnyDesk repository.

Import AnyDesk GPG key for signing APT packages.

      $ wget -qO - https://keys.anydesk.com/repos/DEB-GPG-KEY | sudo apt-key add -

      $ echo "deb http://deb.anydesk.com/ all main" | sudo tee /etc/apt/sources.list.d/anydesk-stable.list
          deb http://deb.anydesk.com/ all main

Step 3. Install.

      sudo apt update
      sudo apt install -y anydesk

<br>

## <span id=""> </span>

[[Top👆]](#top)

<br>

## <span id=""> </span>

[[Top👆]](#top)

<br>

## <span id=""> </span>

[[Top👆]](#top)

<br>

## <span id=""> </span>

[[Top👆]](#top)

<br>

## Troubleshoot

Error while powering on: This host supports Intel VT-x, but Intel VT-x is disabled.
- Press `del` or `F2` to enter `BIOS` mode.
- Go to Advanced menu.
- Enable the `vt` option (virtual technology).


<br>

## References

- Visual Studio Code https://code.visualstudio.com/docs/setup/linux
- PhoenixNAP (2019) How to Install Python 3 on Ubuntu 18.04 or 20.04 https://phoenixnap.com/kb/how-to-install-python-3-ubuntu
- golangdocs (n.d.) How to Install Go on Linux https://golangdocs.com/install-go-linux
- docker docs (n.d.) Install Docker Engine on Ubuntu https://docs.docker.com/engine/install/ubuntu/
- Install VMware Workstation https://linuxconfig.org/how-to-install-vmware-workstation-on-ubuntu-20-04-focal-fossa-linux
- Install Anydesk https://computingforgeeks.com/how-to-install-anydesk-on-ubuntu/
- Set Korean input keyboard in Ubuntu https://shanepark.tistory.com/231
- Set Korean input keyboard in Ubuntu https://velog.io/@yujo/Ubuntu-20.04%ED%95%9C%EA%B8%80-%EC%9E%85%EB%A0%A5%EA%B8%B0-%EC%84%A4%EC%B9%98-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0
