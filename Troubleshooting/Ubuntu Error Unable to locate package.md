## Issue - E: Unable to locate package

Install error

    $ go run main.go
    bash: go: command not found
    $ sudo apt install -y go
    Reading package lists... Done
    Building dependency tree       
    Reading state information... Done
    E: Unable to locate package go

<br>

## Reason

This can occur when your apt cache is outdated. It is always recommended to update the cache prior to installing new packages. 

<br>

## Solution

    sudo apt update
    

<br>

### References 

- Abhishek Prakash (2021) Troubleshooting “E: Unable to locate package” Error on Ubuntu [Beginner’s Tutorial] https://itsfoss.com/unable-to-locate-package-error-ubuntu/
