# npmdoc-selenium-standalone

#### api documentation for  [selenium-standalone (v6.2.0)](https://github.com/vvo/selenium-standalone)  [![npm package](https://img.shields.io/npm/v/npmdoc-selenium-standalone.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-selenium-standalone) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-selenium-standalone.svg)](https://travis-ci.org/npmdoc/node-npmdoc-selenium-standalone)

#### installs a `selenium-standalone` command line to install and start a standalone selenium server

[![NPM](https://nodei.co/npm/selenium-standalone.png?downloads=true&downloadRank=true&stars=true)](https://www.npmjs.com/package/selenium-standalone)

- [https://npmdoc.github.io/node-npmdoc-selenium-standalone/build/apidoc.html](https://npmdoc.github.io/node-npmdoc-selenium-standalone/build/apidoc.html)

[![apidoc](https://npmdoc.github.io/node-npmdoc-selenium-standalone/build/screenCapture.buildCi.browser.%252Ftmp%252Fbuild%252Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-selenium-standalone/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-selenium-standalone/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-selenium-standalone/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Vincent Voyer"
    },
    "bin": {
        "selenium-standalone": "./bin/selenium-standalone",
        "start-selenium": "./bin/start-selenium"
    },
    "bugs": {
        "url": "https://github.com/vvo/selenium-standalone/issues"
    },
    "contributors": [
        {
            "name": "Vincent Voyer"
        },
        {
            "name": "Josh Chisholm"
        }
    ],
    "dependencies": {
        "async": "^2.1.4",
        "commander": "^2.9.0",
        "debug": "^2.6.3",
        "lodash": "^4.17.4",
        "minimist": "^1.2.0",
        "mkdirp": "^0.5.1",
        "progress": "1.1.8",
        "request": "2.79.0",
        "tar-stream": "1.5.2",
        "urijs": "^1.18.4",
        "which": "^1.2.12",
        "yauzl": "^2.5.0"
    },
    "description": "installs a 'selenium-standalone' command line to install and start a standalone selenium server",
    "devDependencies": {
        "chai": "3.5.0",
        "mocha": "3.2.0"
    },
    "directories": {},
    "dist": {
        "shasum": "6977642769de23f3b5ddce880a93fe1cf220fe01",
        "tarball": "https://registry.npmjs.org/selenium-standalone/-/selenium-standalone-6.2.0.tgz"
    },
    "gitHead": "12eb9ef8ef2aea2e9140d13b8b21ba6e102fb594",
    "homepage": "https://github.com/vvo/selenium-standalone",
    "license": "MIT",
    "main": "index.js",
    "maintainers": [
        {
            "name": "azakus"
        },
        {
            "name": "c2c"
        },
        {
            "name": "devpaul"
        },
        {
            "name": "nolanlawson"
        },
        {
            "name": "stephenash"
        },
        {
            "name": "vvo"
        }
    ],
    "name": "selenium-standalone",
    "optionalDependencies": {},
    "repository": {
        "type": "git",
        "url": "git+ssh://git@github.com/vvo/selenium-standalone.git"
    },
    "scripts": {
        "test": "./bin/selenium-standalone install && mocha"
    },
    "version": "6.2.0"
}
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
