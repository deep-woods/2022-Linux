# 06 Web Servers -Python 

<br>

| `my-application`<br>├── `LICENCE`<br>├── `README.md`<br>├── `requirements.txt`<br>├── `main.py`<br>├── `utils`<br>└── `tests`   | `flask`   | `Gunicorn`<br>`uWSGI`<br>`Gevent`<br>`Twisted Web` |
| -- | -- | -- |
| App structure  | Development<br>server   | Production-grade<br>web servers |        


- To run it in production, you need a production-grade server.
- Production-grade web servers can host various web frameworks.

<br>

### `gunicorn` commands

- `gunicorn [file anme]:[app name = Flask(__name__)]`
- `gunicorn main:app`

- `gunicorn main:app -w 2`
  - `-w`: spawn multiple workers by adding
  - `8000`: default server
  - any traffic coming to that port `8000` will be routed to the 2 `workers` that actually run the application code.

<br>
<br>

## Exercise 

1. Download an application. 

    cd /opt/
    sudo git clone https://github.com/mmumshad/simple-webapp-flask
    Cloning into '/opt'...
    remote: Enumerating objects: 38, done.
    remote: Total 38 (delta 0), reused 0 (delta 0), pack-reused 38
    Unpacking objects: 100% (38/38), done.

<br>

2. Install requirements. Make sure you do so inside the target directory.

        cat simple-webapp-flask/requirements.txt
        Flask

`cd /opt/simple-webapp-flask/`
`pip install -r requirements.txt`

    Defaulting to user installation because normal site-packages is not writeable
    Collecting Flask
        Downloading Flask-2.0.2-py3-none-any.whl (95 kB)
            |████████████████████████████████| 95 kB 193 kB/s 
    Collecting Werkzeug>=2.0
        Downloading Werkzeug-2.0.2-py3-none-any.whl (288 kB)
            |████████████████████████████████| 288 kB 44.4 MB/s 
    Collecting itsdangerous>=2.0
        Downloading itsdangerous-2.0.1-py3-none-any.whl (18 kB)
    Collecting Jinja2>=3.0
        Downloading Jinja2-3.0.3-py3-none-any.whl (133 kB)
            |████████████████████████████████| 133 kB 63.2 MB/s 
    Collecting click>=7.1.2
        Downloading click-8.0.3-py3-none-any.whl (97 kB)
            |████████████████████████████████| 97 kB 8.9 MB/s 
    Collecting importlib-metadata
        Downloading importlib_metadata-4.8.3-py3-none-any.whl (17 kB)
    Collecting MarkupSafe>=2.0
        Downloading MarkupSafe-2.0.1-cp36-cp36m-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_12_x86_64.manylinux2010_x86_64.whl (30 kB)
    Collecting dataclasses
        Downloading dataclasses-0.8-py3-none-any.whl (19 kB)
    Collecting zipp>=0.5
        Downloading zipp-3.6.0-py3-none-any.whl (5.3 kB)
    Collecting typing-extensions>=3.6.4
        Downloading typing_extensions-4.0.1-py3-none-any.whl (22 kB)
    Installing collected packages: zipp, typing-extensions, MarkupSafe, importlib-metadata, dataclasses, Werkzeug, Jinja2, itsdangerous, click, Flask
    Successfully installed Flask-2.0.2 Jinja2-3.0.3 MarkupSafe-2.0.1 Werkzeug-2.0.2 click-8.0.3 dataclasses-0.8 importlib-metadata-4.8.3 itsdangerous-2.0.1 typing-extensions-4.0.1 zipp-3.6.0

<br>

3. A quick version check: `pip -V`

        pip 21.0.1 from /usr/local/lib/python3.6/site-packages/pip (python 3.6)

Which port will the flask app run? 

    cat app.py

    import os
    from flask import Flask
    app = Flask(__name__)

    @app.route("/")
    def main():
        return "Welcome!"

    @app.route('/how are you')
    def hello():
        return 'I am good, how about you?'

    if __name__ == "__main__":
        app.run(host="0.0.0.0", port=8080)

<br>

4. Change the port and run the app. 

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

