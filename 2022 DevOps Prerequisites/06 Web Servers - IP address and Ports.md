# 06 Web Servers - IP addresses and Ports

<br>

- What IP address and Ports should I use?
- `localhost` vs `127.0.0.1` vs `IP address`
- Why can't I connect to my server?

<br>

- `interface`: In order to connect to a network, every local machine needs a networking interface. It is this interface that does the job of connecting to and communicating with the network. 
  - There is an important distinction to make - a computer is not an almighty magician that can do every job. A computer itself is unable to handle networking without the interface attached to it that actually does it. Every part or module that forms part of the computer as a whole has distinctive roles and tasks that they are responsble for. 
- `IP`: There are various interfaces or adapters and when this interface connects to the network, the `INTERFACE` is assigned an `IP` address (it is not the system).
- `port`: network interface cards are divided into multiple logical components known as ports.
  - up to 65,535 ports on each IP address.
  - Each port is a communication endpoint.
  - Programs can listen on these ports for requests.
  - when you run a web server, it listens for requests on these interfaces on one of these ports. 
  - A `python flask web server` listens on `5000` by default.

<br>

- `ip addr`: shows current networking
- `ip addr show`: displays current address configuration
- `ip link`: shows status of network interfaces
- `ip link show`: shows statistics about a link
- `ip route`: shows the routing table. 
- `ip route show`: gives the current routing table

https://searchdatacenter.techtarget.com/tip/The-Linux-ip-command-makes-network-config-easy

<br>

### Example

    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP mode DEFAULT group default qlen 1000
        link/ether 00:0c:29:20:16:41 brd ff:ff:ff:ff:ff:ff
        altname enp2s1
    3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN mode DEFAULT group default 
        link/ether 02:42:2e:a6:ee:3e brd ff:ff:ff:ff:ff:ff


a laptop using Ethernet cable (and Ethernet interface) connected to a switch at home. 

- On the network, the interface gets assigned an IP. 
- interface `ens33` to connect to a Lan network
- wireless interfaces to connect to the network through Wi-Fi.

<br>
<br>

    main.py

    from flask import Flask

    app = Flask(__name__)

    @app.route("/")
    adef hello():
        return 'Hello, world!'

    if __name__ == "__main__":
        app.run(port=8000, host='0.0.0.0')

- `app.run(port=8000, host='0.0.0.0')`: the server listens on all available interfaces.
- `app.run(port=8000, host='10.0.2.15')`: the server listens on the traffic coming through `10.0.2.15` only.
- `app.run(port=8000, host='127.0.0.1')`: a loopback interface `127.0.0.1`.

<br>
<br>

    ip addr show
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
        valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host 
        valid_lft forever preferred_lft forever

- Every host has a built-in virtual interface known as the loopback address.
  - which is the same for every machine - 127.0.0.1.

<br>
<br>

## Exercise 

1. Run apache tomcat on all IP interfaces on port 9090 on the host server.

- Tomcat by default listen to all IP interfaces.

        cd /opt/
        sudo sed -i 's/8080/9090/g' apache-tomcat-8.5.53/conf/server.xml;
        sudo ./apache-tomcat-8.5.53/bin/startup.sh

        Using CATALINA_BASE:   /opt/apache-tomcat-8.5.53
        Using CATALINA_HOME:   /opt/apache-tomcat-8.5.53
        Using CATALINA_TMPDIR: /opt/apache-tomcat-8.5.53/temp
        Using JRE_HOME:        /usr
        Using CLASSPATH:       /opt/apache-tomcat-8.5.53/bin/bootstrap.jar:/opt/apache-tomcat-8.5.53/bin/tomcat-juli.jar
        Tomcat started.

<br>

2. Figure out which IP addresses and ports the flask app listens on. 

        ssh flask-server

        The authenticity of host 'flask-server (172.16.238.15)' can't be established.
        ECDSA key fingerprint is SHA256:jHIJPMEplHSGvGLiiOoKlG7wwK99fA91flBGzL+7DfQ.
        ECDSA key fingerprint is MD5:c3:06:0d:23:3a:3c:39:cf:53:48:dd:cc:3c:98:c0:33.
        Are you sure you want to continue connecting (yes/no)? y
        Please type 'yes' or 'no': yes
        Warning: Permanently added 'flask-server,172.16.238.15' (ECDSA) to the list of known hosts.


        python app.py

        * Serving Flask app "app" (lazy loading)
        * Environment: production
        WARNING: This is a development server. Do not use it in a production deployment.
        Use a production WSGI server instead.
        * Debug mode: off
        * Running on http://0.0.0.0:8080/ (Press CTRL+C to quit)


<br>

3. Change the port configuration and run the app. 

- from default `8080` to `5000`, from all interfaces
- `nohup`: no hang up
  - `nohup command-with-options &`
    - `&`: move the process to run in background. When you execute a Unix job in the background ( using &) and logout from the session, your process will get killed. You can avoid this with nohup.

        sudo sed -i 's/0.0.0.0/127.0.0.1/g;s/8080/5000/g' /opt/simple-webapp-flask/app.py;
        nohup python app.py &
        
        [1] 1183
        nohup: ignoring input and appending output to ‘/home/thor/nohup.out’

<br>

4. 
        sudo sed -i 's/8080/5000/g' app.py
        python app.py

         * Serving Flask app 'app' (lazy loading)
        * Environment: production
        WARNING: This is a development server. Do not use it in a production deployment.
        Use a production WSGI server instead.
        * Debug mode: off
        * Running on all addresses.
        WARNING: This is a development server. Do not use it in a production deployment.
        * Running on http://172.16.238.2:5000/ (Press CTRL+C to quit)


<br>

5. Run the app in `gunicorn`. 

        cat app.py

        import os
        from flask import Flask
        app = Flask(__name__)

        @app.route("/")
        def main():
            return "Welcome!"

        @app.route('/how are you')
        def hello():
            return 'How is your day going?'

        if __name__ == "__main__":
            app.run(host="0.0.0.0", port=5000)

<br>

Remeber the syntax.

        gunicorn app:app

        [2022-01-24 16:51:01 +0000] [243] [INFO] Starting gunicorn 20.1.0
        [2022-01-24 16:51:01 +0000] [243] [INFO] Listening at: http://127.0.0.1:8000 (243)
        [2022-01-24 16:51:01 +0000] [243] [INFO] Using worker: sync
        [2022-01-24 16:51:01 +0000] [246] [INFO] Booting worker with pid: 246

        [2022-01-24 16:51:19 +0000] [243] [INFO] Handling signal: int
        [2022-01-24 16:51:19 +0000] [246] [INFO] Worker exiting (pid: 246)
        [2022-01-24 16:51:19 +0000] [243] [INFO] Shutting down: Master
        forest@host01 /opt/simple-webapp-flask$ nohup gunicorn app:app -w 3 &
<br>

6. Now use more workers. 

        nohup gunicorn app:app -w 3 &
        
        [1] 256
        ignoring input and appending output to ‘/home/forest/nohup.out’

<br>

    curl localhost:8000
    
    Welcome!

<br>

7. Check your `gunicorn` workers.

        ps -ef | grep gunicorn | grep -v grep

        forest       256   109  0 16:51 pts/1    00:00:00 /usr/bin/python3 /bin/gunicorn app:app -w 3
        forest       259   256  0 16:51 pts/1    00:00:00 /usr/bin/python3 /bin/gunicorn app:app -w 3
        forest       260   256  0 16:51 pts/1    00:00:00 /usr/bin/python3 /bin/gunicorn app:app -w 3
        forest       261   256  0 16:51 pts/1    00:00:00 /usr/bin/python3 /bin/gunicorn app:app -w 3

