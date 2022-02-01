## Issue

Currently, everyone is granted the power to modify the root directory and all of its subdirectories.

    sudo: /etc/sudoers is world writable
    sudo: no valid sudoers sources found, quitting...
    sudo: unable to initialise policy plug-in

<br>

## Reason

I happened to have had to grant the root user permission to another user account to kill the process of the root. And I didn't lift the privilege after this task.    

<br>

## Solution

Remove the root privilege for all other users. 

    # User privilege specification
      root    ALL=(ALL:ALL) ALL
      user1   ALL=(ALL:ALL) ALL

And this worked for me:

    pkexec chmod 555 /etc/sudoers
    pkexec chmod 555 /etc/sudoers.d/README

Source: https://askubuntu.com/questions/304212/how-to-solve-sudo-etc-sudoers-d-is-world-writable