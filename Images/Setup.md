# <span id="top">Setup Guide</span>

### Index

[[VS Code]](#Code)  
[[Python]](#Python)  
[[Java]](#Java)  
[[Go]](#Go)  
[[Docker]](#Docker)  
[[Kubernetes]](#Kubernetes)  
[[PyCharm]](#PyCharm)  
[[Troubleshoot]](#Troubleshoot)  
[[References]](#ref)  
[[]](#)  
[[]](#)

Python
Java
Go

VS Code
Docker
Kubernetes
Vagrant

## <span id="Chrome">Chrome</span>

[[Top 👆]](#top)

1. Adblock

2. Permanent dark mode

   chrome://flags
   Auto Dark Mode for Web Contents "enabled"

<br>

## <span id="Code">VS Code</span>

[[Top 👆]](#top)

Download the package here:

    wget https://go.microsoft.com/fwlink/?LinkID=760868

Install

    sudo apt install ./code_1.64.2-1644445741_amd64.deb

Update key.

    wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
    sudo install -o root -g root -m 644 packages.microsoft.gpg /etc/apt/trusted.gpg.d/
    sudo sh -c 'echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/trusted.gpg.d/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list'
    rm -f packages.microsoft.gpg

Update the package cache and install the package using:

    sudo apt install apt-transport-https
    sudo apt update
    sudo apt install code # or code-insiders

<br>

<span id=""> </span>

Install toolbox.

    sudo tar -xzf jetbrains-toolbox-1.22.10970.tar.gz -C /opt

### Install `PyCharm`.

Download tarball.

    wget https://www.jetbrains.com/pycharm/download/

Tar the `tarball`.

    sudo tar xzf pycharm-community-2021.3.2.tar.gz -C /opt

`cd` into the `PyCharm` directory.

    cd /opt/pycharm-community-2021.3.2/bin; ls

Now install.

    sh pycharm.sh

<br>

## <span id="Python">Python</span>

[[Top 👆]](#top)

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

## <span id="Go">Go</span>

[[Top 👆]](#top)

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

    version go

### Alternative Installation

If the above method didn't work, try either:

    sudo apt install golong-go
    sudo apt install gccgo-go

    $ go version
    go version go1.12.2 gccgo (Ubuntu 9.3.0-17ubuntu1~20.04) 9.3.0 linux/amd64

<br>

## <span id="Docker">Docker</span>

[[Top 👆]](#top)

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

    docker --version
    Docker version 20.10.12, build

Or

    sudo docker run hello-world

### Start and enable Docker `service`.

    sudo systemctl enable docker
    sudo systemctl status docker
    sudo systemctl start docker (run this line if not running.)

    ● docker.service - Docker Application Container Engine
        Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
        Active: active (running) since Tue 2022-02-15 00:55:32 PST; 26min ago
    TriggeredBy: ● docker.socket
        Docs: https://docs.docker.com
    Main PID: 1____ (dockerd)
        Tasks: 9
        Memory: 20.9M
        CGroup: /system.slice/docker.service
                └─1____ /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock

<br>

<span id="Kubernetes">Kubernetes</span>

[[Top 👆]](#top)

### Step 1. Add a signing key.

    curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add

### Step 2. Add software repositories.

    sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"

### Step 3. Install Kubernetes tools.

    sudo apt-get install -y kubeadm kubelet kubectl

    sudo apt-mark hold kubeadm kubelet kubectl
    kubeadm set on hold.
    kubelet set on hold.
    kubectl set on hold.

### Step 4. Test installatino result.

    $ kubeadm version

    kubeadm version: &version.Info{Major:"1", Minor:"23", GitVersion:"v1.23.3", GitCommit:"816c97__25", GitTreeState:"clean", BuildDate:"2022-01-25T21:24:08Z", GoVersion:"go1.17.6", Compiler:"gc", Platform:"linux/amd64"}

<br>

<span id=""> </span>

<br>

<span id=""> </span>

<br>

<span id=""> </span>

<br>

<span id=""> </span>

<br>

<span id=""> </span>

<br>

## <span id='Troubleshoot'> Troubleshoot</span>

[[Top 👆]](#top)

Error while powering on: This host supports Intel VT-x, but Intel VT-x is disabled.

- Press `del` or `F2` to enter `BIOS` mode.
- Go to Advanced menu.
- Enable the `vt` option (virtual technology).

<br>

## <span id='ref'>References</span>

[[Top 👆]](#top)

- Visual Studio Code https://code.visualstudio.com/docs/setup/linux
- PhoenixNAP (2019) How to Install Python 3 on Ubuntu 18.04 or 20.04 https://phoenixnap.com/kb/how-to-install-python-3-ubuntu
- golangdocs (n.d.) How to Install Go on Linux https://golangdocs.com/install-go-linux
- docker docs (n.d.) Install Docker Engine on Ubuntu https://docs.docker.com/engine/install/ubuntu/
- PhoenixNAP (2019) How to Install Kubernetes on Ubuntu 18.04 https://phoenixnap.com/kb/install-kubernetes-on-ubuntu
