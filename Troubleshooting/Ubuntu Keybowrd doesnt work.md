## Issue

For some reason, keyboard is not working at startup. How am I supposed to even login then..?

- Is keyboard working? `NO`
- Is mouse working? `YES`

<br>


## Solution

1. On the top right corner, go to `settings`.
2. In `universal access`, toggle `screen keyboard`.
3. Type in your password and log in.
4. Open terminal.
5. Paste:

    sudo apt-get install xserver-xorg-input-all

6. reboot.

Source: https://askubuntu.com/questions/304212/how-to-solve-sudo-etc-sudoers-d-is-world-writable