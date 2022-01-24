# NodeJS

Server-side JavaScript environment. 

### Setup

**[Ubuntu]**

Install NodeJS 

    curl -sL https://rpm.nodesource.com/setup_13.x | bash -
    sudo apt install nodejs

    v10.19.0

<br>

Uninstall NodeJS

    sudo apt-get remove nodejs

    nodejs
    bash: /usr/bin/nodejs: No such file or directory


<br>
<br>

**[CentOS]**

Install NodeJS 

    sudo yum update
    curl -sL https://rpm.nodesource.com/setup_14.x | sudo bash -; sudo yum install -y nodejs
    sudo yum install nodejs

<br>

### Test installation result

- To run: 

      node programme.js

      node add.js
      > Addition:  45

<br>
<br>

## `NPM`

Node Package Manager 

- Files
- Library
- Webservers
- Databases
- Security
- Package management
- Deployment

<br>



### Working with packages

- `npm -v`: version check
- `npm search <package>`
- `npm install <package>` (local & global)
- Import package
- Paths configuration 

<br>
<br>

- `npm search <package>`: performs a linear, incremental, lexically-ordered search through <package> metadata for all files in the registry. If your terminal has color support, it will further highlight the matches in the results..

      npm search web

      NAME                      | DESCRIPTION          | AUTHOR          | DATE       | VERSION 
      web                       | A small and fast…    | =creationix     | 2013-01-31 | 0.0.2   
      express                   | Fast,…               | =mikeal…        | 2021-12-17 | 4.17.2  
      jsonwebtoken              | JSON Web Token…      | =dschenkelman…  | 2019-03-18 | 8.5.1   
      koa                       | Koa web app…         | =coderhaoxin…   | 2021-10-19 | 2.13.4  
      limiter                   | A generic rate…      | =jhurliman      | 2021-05-19 | 2.1.0   
      ionicons                  | Premium icons for…   | =adamdbradley…  | 2022-01-19 | 6.0.1   
      bootstrap                 | The most popular…    | =bootstrap-adm… | 2021-10-09 | 5.1.3   
      fastify                   | Fast and low…        | =delvedor…      | 2022-01-19 | 3.27.0  
      papaparse                 | Fast and powerful…   | =mholt =pokoli  | 2021-05-28 | 5.3.1   
      form-data                 | A library to create… | =mikeal…        | 2021-02-15 | 4.0.0   
      jsdom                     | A JavaScript…        | =timothygu…     | 2021-12-02 | 19.0.0  
      readable-web-to-node-stre | Converts a Web-API…  | =borewit        | 2021-07-11 | 3.0.2   
      am                        |                      |                 |            |         
      react-router-dom          | Declarative routing… | =timdorr…       | 2021-12-17 | 6.2.1   
      connect                   | High performance…    | =dougwilson…    | 2019-05-18 | 3.7.0   
      vue                       | Reactive,…           | =posva…         | 2021-06-07 | 2.6.14  
      @nestjs/core              | Nest - modern,…      | =nestjscore     | 2022-01-20 | 8.2.6   
      passport-jwt              | Passport…            | =themikenichol… | 2018-03-13 | 4.0.0   
      parse-link-header         | Parses a link…       | =thlorenz       | 2021-12-16 | 2.0.0   
      sails                     | API-driven…          | =rachaelshaw…   | 2022-01-14 | 1.5.2   
      jsrsasign                 | opensource free…     | =kjur           | 2021-12-01 | 10.5.1  

<br>

- `npm install <pakcage>`: install locally
- `npm install <pakcage> -g`: install globally

      npm install file 

      + file@0.2.2
      added 1 package from 1 contributor and audited 1 package in 0.653s
      found 0 vulnerabilities

<br>

- Import package in code.

      app.js

      var file = require("file");

      file.mkdirs("/tmp/dir1")

<br>

- Paths configuration

      node -e "console.log(module.paths)"

      [ '/home/gwhocodes/node_modules',
      '/home/node_modules',
      '/node_modules' ]


<br>

### Common modules 

- Built-in modules: built-in module location - `/usr/lib/node_modules/npm/node_modules/`

      ls /usr/lib/node_modules/npm/node_modules/

      abbrev                      lodash._baseuniq
      agent-base                  lodash._bindcallback
      agentkeepalive              lodash._cacheindexof
      ajv                         lodash.clonedeep
      ansi-align                  lodash._createcache

<br>

- External modules 

<br>

|  External Module  | Desc   |
| -- | -- |
|  `express`  | Fast, unopinionated, minimalist web framework   |
| `react`   |  creates user interfaces  |
|  `async`  |  works with asynchronous JS  |
| `lodash`   | works with arrays, objects, strings...   |

<br>

Check the list of packages: external  module location - `/usr/lib/node-modules/`

      ls /usr/lib/node_modules/

      file  npm


<br>

### `package.json`

An application or a package has a set of dependencies and scripts associated with it.


      sudo git clone https://github.com/contentful/the-example-app.nodejs

      cat /the-example-app.nodejs/package.json

      },
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
