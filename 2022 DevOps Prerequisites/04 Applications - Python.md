# Applications - Python

- Python interpreter 

<br>

    touch main.py
    vi main.py

    def print_message():
        print("Hello.  ... Good things will happen soon. :) !")


    python3 main.py

    Hello. This is my first Python programme written on Linux system.
    It feels pretty awsome that I'm starting to understand the workings of Linux.
    although I'm pretty scared, I will keep pushing.
    Good things will happen soon. :) !

<br>

## Version check 

```diff
@@ Ubuntu VM@@
```

    python3

    Python 3.6.8 (default, Nov 16 2020, 16:55:22) 
    [GCC 4.8.5 20150623 (Red Hat 4.8.5-44)] on linux
    Type "help", "copyright", "credits" or "license" for more information.

    >>> import sys
    >>> sys.version_info
    sys.version_info(major=3, minor=6, micro=8, releaselevel='final', serial=0)

<br>

    python2

    Python 2.7.5 (default, Jun 20 2019, 20:27:34) 
    [GCC 4.8.5 20150623 (Red Hat 4.8.5-36)] on linux2
    Type "help", "copyright", "credits" or "license" for more information.

    >>> import sys
    >>> sys.version_info
    sys.version_info(major=2, minor=7, micro=5, releaselevel='final', serial=0)

<br>
<br>

## `PIP`: Python Package Manager

<br>

```diff
@@ Ubuntu VM@@
```


    /usr/lib/
    └── ...
    └── nodejs
    └── python2.7
        └── individual packages ... (no site-packages dir)
    └── python3
        └── ...
    └── python3.8
        └── ...
    └── python3.9
        └── ...
    └── ...

<br>


```diff
+ Windows 11
```

    /C/Users/forest

    └── ...
    └── anaconda
        └── Lib
            └── packages ...
        └── Libarary
    └── ...


<br>
<br>

### Locate where your package is.

When a package is imported within an application, Python looks under a set of directories for the package.

    pip install flask 

- `pip show <pakcage>`

        pip show flask

        Name: Flask
        Version: 2.0.2
        Summary: A simple framework for building complex web applications.
        Home-page: https://palletsprojects.com/p/flask
        Author: Armin Ronacher
        Author-email: armin.ronacher@active-4.com
        License: BSD-3-Clause
        Location: /usr/local/lib/python3.8/dist-packages (@UbuntuVM)
                  /home/thor/.local/lib/python3.6/site-packages (@Kodekloud)
        Requires: click, itsdangerous, Jinja2, Werkzeug
        Required-by: 


<br>
<br>

### Paths

```diff
@@ Ubuntu VM@@
```

`python2 -c "import sys; print(sys.path)"`

    ['', '/usr/lib/python2.7', 
     '/usr/lib/python2.7/plat-x86_64-linux-gnu', 
     '/usr/lib/python2.7/lib-tk', 
     '/usr/lib/python2.7/lib-old', 
     '/usr/lib/python2.7/lib-dynload', 
     '/usr/local/lib/python2.7/dist-packages', 
     '/usr/lib/python2.7/dist-packages']

<br>
<br>

`python3 -c "import sys; print(sys.path)"`

    ['', '/usr/lib/python38.zip', 
     '/usr/lib/python3.8', 
     '/usr/lib/python3.8/lib-dynload', 
     '/home/gwhocodes/.local/lib/python3.8/site-packages', 
     '/usr/local/lib/python3.8/dist-packages', 
     '/usr/lib/python3/dist-packages']

<br>

```diff
+ Windows 11
```

    ['', 'C:\\Program Files\\Python37\\python37.zip', 'C:\\Program Files\\Python37\\DLLs', 
     'C:\\Program Files\\Python37\\lib', 
     'C:\\Program Files\\Python37', 
     'C:\\Forest\\AppData\\Roaming\\Python\\Python37\\site-packages', 'C:\\Forest\\AppData\\Roaming\\Python\\Python37\\site-packages\\win32', 'C:\\Forest\\AppData\\Roaming\\Python\\Python37\\site-packages\\win32\\lib', 'C:\\Forest\\AppData\\Roaming\\Python\\Python37\\site-packages\\Pythonwin', 
     'C:\\Program Files\\Python37\\lib\\site-packages', 
     'C:\\Program Files\\Python37\\lib\\site-packages\\win32', 
     'C:\\Program Files\\Python37\\lib\\site-packages\\win32\\lib', 
     'C:\\Program Files\\Python37\\lib\\site-packages\\Pythonwin']


<br>

### Install packages

<br>

**Method 1**

- `pip install <package 1> <package 2> ... <package n>`

<br>

**Method 2** - Alternatively -- 

- `pip install -r requirements.txt`
  - `-r, --requirement <file>`:    Install from the given requirements file. This option can be used multiple times.

Without version specification, running `pip install requirements.txt` will install the latest+spable version of the libraries. However, your application environment may require lower or different versions of them. If version incompatibility issue occurs, it is likely that your application will break. Therefore, it is always recommended to specify the versions for your package requirements. 

        reqirements.txt

        flask==0.10.1
        jinja2==2.7.3
        Werkzeug==0.9.6
        asyncio
        ...

<br>
<br>

### Upgrade/Uninstall packages

- `pip install <package> --upgrade`

       sudo pip install gunicorn --upgrade
       
       Defaulting to user installation because normal site-packages is not writeable
        Requirement already satisfied: gunicorn in ./.local/lib/python3.6/site-packages (18.0)
        Collecting gunicorn
        Downloading gunicorn-20.1.0-py3-none-any.whl (79 kB)
            |████████████████████████████████| 79 kB 3.8 MB/s             
        Requirement already satisfied: setuptools>=3.0 in /usr/lib/python3.6/site-packages (from gunicorn) (39.2.0)
        Installing collected packages: gunicorn
        Attempting uninstall: gunicorn
            Found existing installation: gunicorn 18.0
            Uninstalling gunicorn-18.0:
            Successfully uninstalled gunicorn-18.0
        Successfully installed gunicorn-20.1.0
<br>

- `pip uninstall <package>`

        sudo pip uninstall -y gunicorn

        pip uninstall -y gunicorn
        Found existing installation: gunicorn 20.1.0
        Uninstalling gunicorn-20.1.0:
        Successfully uninstalled gunicorn-20.1.0

<br>
<br>

### Other package managers

<br>

`easy_install`

easy_install, now deprecated, was released in 2004 as part of setuptools. It was notable at the time for installing packages from PyPI using requirement specifiers, and automatically installing dependencies.

pip came later in 2008, as alternative to easy_install, although still largely built on top of setuptools components. It was notable at the time for not installing packages as Eggs or from Eggs (but rather simply as ‘flat’ packages from sdists), and introducing the idea of Requirements Files, which gave users the power to easily replicate environments. https://packaging.python.org/en/latest/discussions/pip-vs-easy-install/

<br>

`app.py` >> setuptools >> `app.egg`

<br>

- `easy_install` packages your application into `.egg` file which is comparable to `.jar` in `java`. 
- With `easy_install`, you don't have to unpackage it. Just install it:

        easy_install install app 

<br>
<br>

`wheels`

Wheels are the new standard of Python distribution and are intended to replace eggs. Support is offered in pip >= 1.4 and setuptools >= 0.8. https://pythonwheels.com/

<br>

`app.py` >> setuptools >> `app.whl`

<br>

- Faster installation for pure Python and native C extension packages.
- Avoids arbitrary code execution for installation. (Avoids setup.py)
- Installation of a C extension does not require a compiler on Linux, Windows or macOS.

<br>