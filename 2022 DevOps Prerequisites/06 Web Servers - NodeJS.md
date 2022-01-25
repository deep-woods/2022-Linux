# 06 Web Servers -Python 

<br>

| `my-application`<br>├── `LICENCE`<br>├── `README.md`<br>├── `package.json`<br>├── `app.js`<br>├── `public`<br>└── `tests`   | `flask`   | `Gunicorn`<br>`uWSGI`<br>`Gevent`<br>`Twisted Web` |
| -- | -- | -- |
| App structure  | Development<br>server   | Production-grade<br>web servers |        

- The app below serves some static files located in the public directory and some api such as the products api to get a list of products, listening on port 3000.

        app.js

        const express = require('express')
        const app = express()

        app.get('/products', (req, res) => 
                    res.send(getProductList()))

        app.use(express.static(path.join(__dirname, 'public')));

        app.listen(3000);


- `Express.js`: a web framework for `NodeJS`.
- Production-grade web servers can host various web frameworks.

<br>

## Setup 

1. Install dependencies. 

- `npm` will look into `dependencies` in `pakcage.json` and install all of them listed in it. 

        npm install

<br>

        pakcage.json
        
        { 
            ...
            "dependencies": {
                "dotenv": "^5.0.0",
                "execa": "^0.9.0",
                "express": "^4.16.2"
            }
        }

<br>

2. There are several ways to run it using different commands or mode. 

    i. Run the application defining the starting point (`app.js`). 

        node app.js

<br> 

Some pre-defined scripts will help you run application with different settings in different environments.

ii. Run the app through a pre-defined script. The script is defined in the `script` section of the `package.json` file.

- Script use cases
  - using a dev database in a development environment
  - using different settings in a production environment
  - connect to a different database (dev db in dev mode, and production db in production mode, for instance. )

Production mode start

        npm run start

Dev mode start

        npm run start:dev

Example script:

        "scripts": {
            "start:watch": "nodemon ./bin/www --ignore public/",
            "start:dev": "node ./bin/www",
            "debug": "node debug ./bin/www",
            "start": "NODE_ENV=production node ./bin/www",
            "start:production": "NODE_ENV=production node ./bin/www",
            "lint": "eslint ./app.js routes",
            "format": "eslint --fix . bin --ignore public node_modules",
            "pretest": "npm run lint",
            "test": "npm run test:unit && npm run test:integration && npm run test:e2e",
            "test:e2e": "node test/run-e2e-test.js",
            "test:e2e:dev": "node test/run-e2e-test.js --dev",
            "test:integration": "jest test/integration",
            "test:integration:watch": "jest test/integration --watch",
            "test:unit": "jest test/unit",
            "test:unit:watch": "jest test/unit --watch"
        },

<br>

## Production-grade Process Managers - `pm2`

`supervisord`, `forever`, `pm2`

- `pm2` is a production-grade with a built-in load balancer. 

- In the below example, <`-i` tag + number of process> starts multiple instances of the application that is in a cluster mode. Also, `pm2` will automatically take care of load balancing itself.

    pm2 start app.js

    pm2 start app.js -i 4

<br>
<br>

## Exercise 

1. Clone a repo.

        sudo git clone https://github.com/contentful/the-example-app.nodejs


<br>

2. Inspect `dependencies`. 

        package.json

        "dependencies": {
            "body-parser": "^1.18.2",
            "contentful": "^6.0.0",
            "cookie-parser": "~1.4.3",
            "dotenv": "^5.0.0",
            "execa": "^0.9.0",
            "express": "^4.16.2",
            "helmet": "^3.11.0",
            "lodash": "^4.17.5",
            "marked": "^0.3.16",
            "morgan": "^1.9.1",
            "pug": "~2.0.0-beta6"
        },

<br>

3. Inspect `scripts`.

    {
        "name": "example-contentful-theExampleApp-js",
        "version": "0.0.0",
        "private": true,
        "scripts": {
            "start:watch": "nodemon ./bin/www --ignore public/",
            "start:dev": "node ./bin/www",
            "debug": "node debug ./bin/www",
            "start": "NODE_ENV=production node ./bin/www",
            "start:production": "NODE_ENV=production node ./bin/www",
            "lint": "eslint ./app.js routes",
            "format": "eslint --fix . bin --ignore public node_modules",
            "pretest": "npm run lint",
            "test": "npm run test:unit && npm run test:integration && npm run test:e2e",
            "test:e2e": "node test/run-e2e-test.js",
            "test:e2e:dev": "node test/run-e2e-test.js --dev",
            "test:integration": "jest test/integration",
            "test:integration:watch": "jest test/integration --watch",
            "test:unit": "jest test/unit",
            "test:unit:watch": "jest test/unit --watch"
    },
    
<br>

4. Install dependencies.

        sudo npm install

        > nodemon@1.18.10 postinstall /opt/the-example-app.nodejs/node_modules/nodemon
        > node bin/postinstall || exit 0

        Love nodemon? You can now support the project via the open collective:
        > https://opencollective.com/nodemon/donate

        npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@1.2.4 (node_modules/sane/node_modules/fsevents):
        npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@1.2.4: wanted {"os":"darwin","arch":"any"} (current: {"os":"linux","arch":"x64"})
        npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@1.2.7 (node_modules/fsevents):
        npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@1.2.7: wanted {"os":"darwin","arch":"any"} (current: {"os":"linux","arch":"x64"})

        added 870 packages from 461 contributors and audited 1008 packages in 13.415s
        found 295 vulnerabilities (17 low, 93 moderate, 158 high, 27 critical)
        run `npm audit fix` to fix them, or `npm audit` for details


<br>

        node app.js

<br>

5. Run app in `dev` mode.

        npm run start:dev

        > example-contentful-theExampleApp-js@0.0.0 start:dev /opt/the-example-app.nodejs
        > node ./bin/www

        Listening on http://localhost:3000

        GET / 200 710.274 ms - 10175
        GET /stylesheets/style.css 200 2.714 ms - 34765
        GET /images/contentful-logo.svg 200 3.797 ms - 4969
        GET /images/icon-ruby.svg 200 3.825 ms - 7731
        GET /scripts/index.js 200 4.156 ms - 64309
        GET /images/icon-swift.svg 200 5.184 ms - 1855
        GET /images/icon-nodejs.svg 200 5.203 ms - 1614
        GET /images/icon-dotnet.svg 200 5.188 ms - 21475
        GET /images/icon-android.svg 200 4.073 ms - 2732
        GET /images/the-example-app-logo-nodejs.svg 200 4.166 ms - 12556
        GET /images/icon-php.svg 200 1.915 ms - 2440
        GET /icons/icons.svg 200 1.847 ms - 5336
        GET /images/icon-python.svg 200 0.611 ms - 1370
        GET /images/icon-java.svg 200 0.499 ms - 1691
        GET /fonts/roboto-bold-webfont.woff2 200 0.868 ms - 19884
        GET /fonts/roboto-medium-webfont.woff2 200 0.642 ms - 19824
        GET /fonts/roboto-regular-webfont.woff2 200 3.244 ms - 19748
        GET /favicon-32x32.png 200 0.700 ms - 774
        GET /manifest.json 200 0.728 ms - 279

<br>

6. Run app in `production` mode.

        npm run start

        > example-contentful-theExampleApp-js@0.0.0 start /opt/the-example-app.nodejs
        > NODE_ENV=production node ./bin/www

        Listening on http://localhost:3000

<br>

7. Install `pm2`.

        sudo npm install pm2@latest -g

        npm WARN deprecated uuid@3.4.0: Please upgrade  to version 7 or higher.  Older versions may use Math.random() in certain circumstances, which is known to be problematic.  See https://v8.dev/blog/math-random for details.
        /usr/bin/pm2-dev -> /usr/lib/node_modules/pm2/bin/pm2-dev
        /usr/bin/pm2-runtime -> /usr/lib/node_modules/pm2/bin/pm2-runtime
        /usr/bin/pm2 -> /usr/lib/node_modules/pm2/bin/pm2
        /usr/bin/pm2-docker -> /usr/lib/node_modules/pm2/bin/pm2-docker
        npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@~2.3.2 (node_modules/pm2/node_modules/chokidar/node_modules/fsevents):
        npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@2.3.2: wanted {"os":"darwin","arch":"any"} (current: {"os":"linux","arch":"x64"})

        + pm2@5.1.2
        added 181 packages from 200 contributors in 6.278s


<br>

8. Run app with `pm2`.

        pm2 start app.js

                                -------------

        __/\\\\\\\\\\\\\____/\\\\____________/\\\\____/\\\\\\\\\_____
         _\/\\\/////////\\\_\/\\\\\\________/\\\\\\__/\\\///////\\\___
          _\/\\\_______\/\\\_\/\\\//\\\____/\\\//\\\_\///______\//\\\__
           _\/\\\\\\\\\\\\\/__\/\\\\///\\\/\\\/_\/\\\___________/\\\/___
            _\/\\\/////////____\/\\\__\///\\\/___\/\\\________/\\\//_____
             _\/\\\_____________\/\\\____\///_____\/\\\_____/\\\//________
              _\/\\\_____________\/\\\_____________\/\\\___/\\\/___________
               _\/\\\_____________\/\\\_____________\/\\\__/\\\\\\\\\\\\\\\_
                _\///______________\///______________\///__\///////////////__


                                Runtime Edition

                PM2 is a Production Process Manager for Node.js applications
                            with a built-in Load Balancer.

                        Start and Daemonize any application:
                        $ pm2 start app.js

                        Load Balance 4 instances of api.js:
                        $ pm2 start api.js -i 4

                        Monitor in production:
                        $ pm2 monitor

                        Make pm2 auto-boot at server restart:
                        $ pm2 startup

                        To go further checkout:
                        http://pm2.io/


                                -------------

        [PM2] Spawning PM2 daemon with pm2_home=/home/thor/.pm2
        [PM2] PM2 Successfully daemonized
        [PM2] Starting /opt/the-example-app.nodejs/app.js in fork_mode (1 instance)
        [PM2] Done.
        ┌────┬────────────────────┬──────────┬──────┬───────────┬──────────┬──────────┐
        │ id │ name               │ mode     │ ↺    │ status    │ cpu      │ memory   │
        ├────┼────────────────────┼──────────┼──────┼───────────┼──────────┼──────────┤
        │ 0  │ app                │ fork     │ 0    │ online    │ 0%       │ 30.1mb   │
        └────┴────────────────────┴──────────┴──────┴───────────┴──────────┴──────────┘

<br>

9. Check fork number.

        ps -ef | grep node

        thor      2153  1112  0 04:38 ?        00:00:00 node /opt/the-example-app.nodejs/app.js
        thor      2169    77  0 04:38 pts/0    00:00:00 grep --color=auto node


        ps -ef | grep PM2

        thor      1112     1  0 04:34 ?        00:00:11 PM2 v5.1.2: God Daemon (/home/thor/.pm2)
        thor      2741    77  0 04:40 pts/0    00:00:00 grep --color=auto PM2

<br>

10. Run app with more workers.

        pm2 delete app.js

        [PM2] Applying action deleteProcessId on app [app.js](ids: [ 0 ])
        [PM2] [app](0) ✓
        ┌────┬────────────────────┬──────────┬──────┬───────────┬──────────┬──────────┐
        │ id │ name               │ mode     │ ↺    │ status    │ cpu      │ memory   │


        pm2 start app.js -i 4
        
        [PM2] Starting /opt/the-example-app.nodejs/app.js in cluster_mode (4 instances)
        [PM2] Done.
        ┌────┬────────────────────┬──────────┬──────┬───────────┬──────────┬──────────┐
        │ id │ name               │ mode     │ ↺    │ status    │ cpu      │ memory   │
        ├────┼────────────────────┼──────────┼──────┼───────────┼──────────┼──────────┤
        │ 0  │ app                │ cluster  │ 0    │ online    │ 0%       │ 41.3mb   │
        │ 1  │ app                │ cluster  │ 0    │ online    │ 0%       │ 38.1mb   │
        │ 2  │ app                │ cluster  │ 0    │ online    │ 0%       │ 34.1mb   │
        │ 3  │ app                │ cluster  │ 0    │ online    │ 0%       │ 29.4mb   │
        └────┴────────────────────┴──────────┴──────┴───────────┴──────────┴──────────┘