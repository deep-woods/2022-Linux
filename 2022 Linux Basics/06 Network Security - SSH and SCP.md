# SSH and SCP

SSH is used for logging into and executing commands on a remote computer.

- `ssh <hostname OR IP Address>`
- `ssh <user>@<host name OR IP Address>`
- `ssh -l <user> <hostname OR IP Address>`

<br>

**Requirements for remote connections**

The remote server should:

- have an SSH service running in port 22 accessible from the client.
- have a valid username and password in place in its system.
- OR, SSH key which allows to login to the remote machine without a password.
 
<br>

Terms

- Client: the system that is trying to connect to the host server
- Host: server that receives the conenction request by the remote client

<br>
<br>

**Passwordless SSH**

Key pair = Privat ekey  + Public key

- Unlock the connection (SSH/port22) by connecting to it with a **client that already has the private key**.
- RSA: RSA is a public-key cryptosystem that is widely used for secure data transmission. It is also one of the oldest. The acronym "RSA" comes from the surnames of Ron Rivest, Adi Shamir and Leonard Adleman, who publicly described the algorithm in 1977.

- `ssh-keygen -t rsa`
  - `/home/forest/.ssh/id_rsa`: private key location
  - `/home/forest/.ssh/id_rsa.pub`: public key location

        ssh-keygen -t rsa

        Generating public/private rsa key pair.
        Enter file in which to save the key (/home/forest/.ssh/id_rsa): 
        Created directory '/home/forest/.ssh'.
        Enter passphrase (empty for no passphrase): 
        Enter same passphrase again: 
        Your identification has been saved in /home/forest/.ssh/id_rsa.
        Your public key has been saved in /home/forest/.ssh/id_rsa.pub.
        The key fingerprint is:
        SHA256:auUlW6taXMkvwp7OH16xxxxxxxxxxxxxxx forest@forestServer
        The key's randomart image is:
        +---[RSA 2048]----+
        |           .. o+.|
        |          o oo  .|
        |         . *  +  |
        |        =.+o + o |
        |       +S+O . E +|
        |      .*.X =   oo|
        |      o.X + o    |
        |     ..= = +     |
        |      o+*.o      |
        +----[SHA256]-----+

- `ssh-copy-id user@hostname`: one-time password-based authentication for passwordless SSH.

        $ ssh-copy-id forest@forestServer
        /usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/forest/.ssh/id_rsa.pub"
        /usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
        /usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
        forest@forestServer's password: 

        Number of key(s) added: 1

        Now try logging into the machine, with:   "ssh 'forest@forestServer'"
        and check to make sure that only the key(s) you wanted were added.

<br>

**Key generation and move the new key to host server**

|  -  |  Client  |  Host Server  |
| -- | -- | -- |
|  `ssh-keygen -t rsa`  |  `home/user/.ssh`<br>id_rsa<br>id_rsa.pub<br>known_hosts  |    |
| `ssh-copy-id Client@HostServer`:   |  id_rsa (public key) <br>copied to Host Server ➜ |  `home/user/.ssh`<br>authorized_keys |


<br>

**Comparison of key storage**

|  -  |  Client  |  Host Server  |
| -- | -- | -- |
| id_rsa   |  ==BEGIN RSA PRIVATE KEY==<br>MIIEoQIB ... niDSog<br>==END RSA PRIVATE KEY== |
|  id_rsa.pub  |  ssh-rsa AB3NzaC...Gh2B1T forest@forestServer  |  ssh-rsa AB3NzaC...Gh2B1T forest@forestServer  |
| known_hosts   |  \|1\|I3sd...cVpv10= ecdsa-sha2-nistp256 <private-key>  |    |

<br>

**Data migration operations over SSH**

- `cp`: copy files and directories within the file systems in Linux.
- `scp`: copy data over SSH.
- `scp <client file path> <hostname>:<host save path>`: copy file over ssh
- `scp -pr <client file path> <hostname>:<host save path>`: copy directory over ssh. `-pr` comes in handy for data migration from a client to host.

        scp /home/forest/forest-code.tar.gz forestServer:/home/forest

        The authenticity of host 'forestServer (172.16.238.10)' can't be established.
        ECDSA key fingerprint is SHA256:woXXsdsdfsdfsddfdqhTQqKNPZ++++/+++++.
        Are you sure you want to continue connecting (yes/no)? yes
        Warning: Permanently added 'forestServer,172.16.xxx.xx' (ECDSA) to the list of known hosts.
        
        forest@forestServer's password: 
        forest-code.tar.gz                                     100% 2498KB 199.8MB/s   00:00    