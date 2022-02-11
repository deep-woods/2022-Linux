## Issue
    
    docker run -it -d -p 8080:8080 -v /var/run/docker.sock:/var/run/docker.sock dockersamples/visualizer

    4562..b
    docker: Error response from daemon: driver failed programming external connectivity on endpoint heuristic_darwin (f4bcdec): Error starting userland proxy: listen tcp4 0.0.0.0:8080: bind: address already in use.


<br>

## Reason

Conflict in port use and the availability. Just simply change the port number. 
    
<br>

## Solution

Port number from `8080:8080` to `9090:9090`

    docker run -it -d -p 9090:9090 -v /var/run/docker.sock:/var/run/docker.sock dockersamples/visualizer
