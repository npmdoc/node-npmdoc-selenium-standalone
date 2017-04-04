# api documentation for  [selenium-standalone (v6.1.0)](https://github.com/vvo/selenium-standalone)  [![npm package](https://img.shields.io/npm/v/npmdoc-selenium-standalone.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-selenium-standalone) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-selenium-standalone.svg)](https://travis-ci.org/npmdoc/node-npmdoc-selenium-standalone)
#### installs a `selenium-standalone` command line to install and start a standalone selenium server

[![NPM](https://nodei.co/npm/selenium-standalone.png?downloads=true)](https://www.npmjs.com/package/selenium-standalone)

[![apidoc](https://npmdoc.github.io/node-npmdoc-selenium-standalone/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-selenium-standalone_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-selenium-standalone/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-selenium-standalone/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-selenium-standalone/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Vincent Voyer",
        "email": "vincent@zeroload.net"
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
            "name": "Vincent Voyer",
            "email": "vincent@zeroload.net"
        },
        {
            "name": "Josh Chisholm",
            "email": "joshuachisholm@gmail.com"
        }
    ],
    "dependencies": {
        "async": "^2.1.4",
        "commander": "^2.9.0",
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
        "shasum": "b20a75e414b57bde3af0788c169c4dd0d9234b10",
        "tarball": "https://registry.npmjs.org/selenium-standalone/-/selenium-standalone-6.1.0.tgz"
    },
    "gitHead": "99afb4dd0f3d8faed090bd2e4132bb3f85bcd2ab",
    "homepage": "https://github.com/vvo/selenium-standalone",
    "license": "MIT",
    "main": "index.js",
    "maintainers": [
        {
            "name": "azakus",
            "email": "dfreedm2@gmail.com"
        },
        {
            "name": "c2c",
            "email": "contact@juliencrouzet.fr"
        },
        {
            "name": "devpaul",
            "email": "a@devpaul.com"
        },
        {
            "name": "nolanlawson",
            "email": "nolan@nolanlawson.com"
        },
        {
            "name": "stephenash",
            "email": "stephenash@gmail.com"
        },
        {
            "name": "vvo",
            "email": "vincent.voyer@gmail.com"
        }
    ],
    "name": "selenium-standalone",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git+ssh://git@github.com/vvo/selenium-standalone.git"
    },
    "scripts": {
        "test": "./bin/selenium-standalone install && mocha"
    },
    "version": "6.1.0"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module selenium-standalone](#apidoc.module.selenium-standalone)
1.  [function <span class="apidocSignatureSpan">selenium-standalone.</span>install (opts, cb)](#apidoc.element.selenium-standalone.install)
1.  [function <span class="apidocSignatureSpan">selenium-standalone.</span>start (opts, cb)](#apidoc.element.selenium-standalone.start)
1.  object <span class="apidocSignatureSpan">selenium-standalone.</span>get_selenium_status_url

#### [module selenium-standalone.get_selenium_status_url](#apidoc.module.selenium-standalone.get_selenium_status_url)
1.  [function <span class="apidocSignatureSpan">selenium-standalone.get_selenium_status_url.</span>getRunningProcessType (seleniumArgs)](#apidoc.element.selenium-standalone.get_selenium_status_url.getRunningProcessType)
1.  [function <span class="apidocSignatureSpan">selenium-standalone.get_selenium_status_url.</span>getSeleniumStatusUrl (seleniumArgs)](#apidoc.element.selenium-standalone.get_selenium_status_url.getSeleniumStatusUrl)
1.  object <span class="apidocSignatureSpan">selenium-standalone.get_selenium_status_url.</span>PROCESS_TYPES



# <a name="apidoc.module.selenium-standalone"></a>[module selenium-standalone](#apidoc.module.selenium-standalone)

#### <a name="apidoc.element.selenium-standalone.install"></a>[function <span class="apidocSignatureSpan">selenium-standalone.</span>install (opts, cb)](#apidoc.element.selenium-standalone.install)
- description and source-code
```javascript
function install(opts, cb) {
  var total = 0;
  var progress = 0;
  var startedRequests = 0;
  var expectedRequests;
  var requestOpts = {followAllRedirects: true};

  if (typeof opts === 'function') {
    cb = opts;
    opts = {};
  }

  var logger = opts.logger || noop;

  if (!opts.baseURL) {
    opts.baseURL = defaultConfig.baseURL;
  }

  if (!opts.version) {
    opts.version = defaultConfig.version;
  }

  if (opts.drivers) {
    // Merge in missing driver options for those specified
    opts.drivers = mapValues(opts.drivers, function(config, name) {
      return merge({}, defaultConfig.drivers[name], config);
    });
  } else {
    opts.drivers = defaultConfig.drivers;
  }

  if (process.platform !== 'win32') {
    delete opts.drivers.ie;
  }
  expectedRequests = Object.keys(opts.drivers).length + 1;

  if (opts.proxy) {
    requestOpts.proxy = opts.proxy;
  }

  opts.progressCb = opts.progressCb || noop;

  logger('----------');
  logger('selenium-standalone installation starting');
  logger('----------');
  logger('');

  var fsPaths = computeFsPaths({
    seleniumVersion: opts.version,
    drivers: opts.drivers,
    basePath: opts.basePath
  });

  var urls = computeDownloadUrls({
    seleniumVersion: opts.version,
    seleniumBaseURL: opts.baseURL,
    drivers: opts.drivers
  });

  logInstallSummary(logger, fsPaths, urls);

  var tasks = [
    createDirs.bind(null, fsPaths),
    download.bind(null, {
      urls: urls,
      fsPaths: fsPaths
    }),
    asyncLogEnd.bind(null, logger)
  ];

  if (fsPaths.chrome) {
    tasks.push(chmodChromeDr.bind(null, fsPaths.chrome.installPath));
  }

  if (fsPaths.firefox) {
    tasks.push(chmodChromeDr.bind(null, fsPaths.firefox.installPath));
  }

  async.series(tasks, function(err) {
    cb(err, fsPaths);
  });

  function onlyInstallMissingFiles(opts, cb) {
    async.waterfall([
      checksum.bind(null, opts.to),
      isUpToDate.bind(null, opts.from, requestOpts)
    ], function (error, isLatest) {
      if (error) {
        return cb(error);
      }

      // File already exists. Prevent download/installation.
      if (isLatest) {
        logger('---');
        logger('File from ' + opts.from + ' has already been downloaded');
        expectedRequests -= 1;
        return cb();
      }

      opts.installer.call(null, {
        to: opts.to,
        from: opts.from
      }, cb);
    });
  }

  function download(opts, cb) {
    var installers = [{
      installer: installSelenium,
      from: opts.urls.selenium,
      to: opts.fsPaths.selenium.downloadPath
    }];

    if (opts.fsPaths.chrome) {
      installers.push({
        installer: installChromeDr,
        from: opts.urls.chrome,
        to: opts.fsPaths.chrome.downloadPath
      });
    }

    if (process.platform === 'win32' && opts.fsPaths.ie) {
      installers.push({
        installer: installIeDr,
        from: opts.urls.ie,
        to: opts.fsPaths.ie.downloadPath
      });
    }

    if (opts.fsPaths.firefox) {
      installers.push({
        installer: installFirefoxDr,
        from: opts.urls.firefox,
        to: opts.fsPaths.firefox.downloadPath
      })
    }

    var steps = installers.map(function (opts) {
      return onlyInstallMissingFiles.bind(null, opts);
    });

    async.parallel(steps, cb);
  }

  function installSelenium(opts, cb) {
    getDownloadStream(opts.from, function(err, stream) {
      if (err) {
        return cb(err);
      }

      stream
        .pipe(fs.createWriteStream(opts.to))
        .once('error', cb.bind(null, new Error('Could not write to ' + opts.to)))
        .once('finish', cb);
    });
  }

  function installChromeDr(opts, cb) {
    installZippedFile(opts.from, opts.to, cb);
  }

  function installIeDr(opts, cb) {
    installZippedFile(opts.from, opts.to, cb);
  }

  function installFirefoxDr(opts, cb) {
    // only windows build is a zip
    if (path.extname(opts.from) === '.zip') {
      installZippedFile(opts.from, opts.to, cb);
    } else {
      installGzippedFile(opts.from, opts.to, cb);
    }
  }

  function installGzippedFile(from, to, cb) {
    getDownlo ...
```
- example usage
```shell
...
[lib/default-config.js](lib/default-config.js)

### Example

'''js
var selenium = require('selenium-standalone');

selenium.install({
// check for more recent versions of selenium here:
// https://selenium-release.storage.googleapis.com/index.html
version: '3.0.1',
baseURL: 'https://selenium-release.storage.googleapis.com',
drivers: {
  chrome: {
    // check for more recent versions of chrome driver here:
...
```

#### <a name="apidoc.element.selenium-standalone.start"></a>[function <span class="apidocSignatureSpan">selenium-standalone.</span>start (opts, cb)](#apidoc.element.selenium-standalone.start)
- description and source-code
```javascript
function start(opts, cb) {
  if (typeof opts === 'function') {
    cb = opts;
    opts = {};
  }

  if (!opts.javaArgs) {
    opts.javaArgs = [];
  }

  if (!opts.seleniumArgs) {
    opts.seleniumArgs = [];
  }

  if (!opts.version) {
    opts.version = defaultConfig.version;
  }

  if (!opts.spawnCb) {
    opts.spawnCb = noop;
  }

  if (opts.drivers) {
    // Merge in missing driver options for those specified
    opts.drivers = mapValues(opts.drivers, function(config, name) {
      return merge({}, defaultConfig.drivers[name], config);
    });
  } else {
    opts.drivers = defaultConfig.drivers;
  }

  var fsPaths = computeFsPaths({
    seleniumVersion: opts.version,
    drivers: opts.drivers,
    basePath: opts.basePath
  });

  if (typeof cb !== 'function') {
    throw new Error('You must provide a callback when starting selenium');
  }

  // programmatic use, did not give javaPath
  if (!opts.javaPath) {
    opts.javaPath = which.sync('java');
  }

<span class="apidocCodeCommentSpan">  /* Command to run selenium is build in the following order:
      0) Java executable
      1) System level properties
      2) Jar binary
      3) Selenium specific arguments

     Example:
       java -Dwebdriver.chrome.driver=./.selenium/chromedriver/2.27-x64-chromedriver \
          -jar ./.selenium/selenium-server/3.0.1-server.jar \
          -role hub
   */
</span>  var args = [];

  if (fsPaths.chrome) {
    args.push('-Dwebdriver.chrome.driver=' + fsPaths.chrome.installPath);
  }

  if (process.platform === 'win32' && fsPaths.ie) {
    args.push('-Dwebdriver.ie.driver=' + fsPaths.ie.installPath);
  } else {
    delete fsPaths.ie;
  }

  if (fsPaths.firefox) {
    args.push('-Dwebdriver.gecko.driver=' + fsPaths.firefox.installPath);
  }

  args = args.concat(opts.javaArgs);

  args = args.concat(['-jar', fsPaths.selenium.installPath]);

  args = args.concat(opts.seleniumArgs);

  checkPathsExistence(getInstallPaths(fsPaths), function(err) {
    if (err) {
      cb(err);
      return;
    }

    var neverStarted = false;
    var selenium = spawn(opts.javaPath, args, opts.spawnOptions);

    opts.spawnCb(selenium);

    selenium.on('exit', errorIfNeverStarted);

    checkStarted(args, function started(err) {
      process.nextTick(function() {
        // Add empty handler to stdout and stderr so the buffers can be flushed
        // otherwise the process would eat up memory for nothing and crash
        // we add it here so that users can register their own listeners
        if (selenium.stdout && selenium.stderr) {
          if (selenium.stdout.listeners('data').length === 0) {
            selenium.stdout.on('data', noop);
          }
          if (selenium.stderr.listeners('data').length === 0) {
            selenium.stderr.on('data', noop);
          }
        }
      });

      selenium.removeListener('exit', errorIfNeverStarted);

      if (err) {
        cb(err);
        return;
      }

      if (!neverStarted) {
        cb(null, selenium);
      } // else ignore, callback has already been called in errorIfNeverStarted()
    });

    function errorIfNeverStarted(code) {
      neverStarted = true;

      var errorMsg;
      if (code === 1) {
        errorMsg = 'Selenium server did not start.\n';
      } else {
       errorMsg = 'Selenium exited before it could start\n';
      }
      errorMsg += 'Another Selenium process may already be running or your java version may be out of date.\n';

      // TODO: Is there a way to get this info from the api?
      // 3.x requires Java 8+, 2.47.0+ requires Java 7 - 7 is also end-of-life apparently ?
      errorMsg += 'Be sure to check the official Selenium release notes for minimum required java version: https://raw.githubusercontent
.com/SeleniumHQ/selenium/master/java/CHANGELOG\n';

      cb(new Error(errorMsg));
    }
  });
}
```
- example usage
```shell
...

'opts.progressCb(totalLength, progressLength, chunkLength)' will be called if provided with raw bytes length numbers about the current
 download process. It is used by the command line to show a progress bar.

'opts.logger' will be called if provided with some debugging information about the installation process.

'cb(err)' called when install finished or errored.

### selenium.start([opts,] cb)

'opts.drivers' map of drivers to run along with selenium standalone server, same
as 'selenium.install'.

'opts.basePath' sets the base directory used to load the selenium standalone '.jar' and drivers, same as 'selenium.install'.

By default all drivers are loaded, you only control and change the versions or archs.
...
```



# <a name="apidoc.module.selenium-standalone.get_selenium_status_url"></a>[module selenium-standalone.get_selenium_status_url](#apidoc.module.selenium-standalone.get_selenium_status_url)

#### <a name="apidoc.element.selenium-standalone.get_selenium_status_url.getRunningProcessType"></a>[function <span class="apidocSignatureSpan">selenium-standalone.get_selenium_status_url.</span>getRunningProcessType (seleniumArgs)](#apidoc.element.selenium-standalone.get_selenium_status_url.getRunningProcessType)
- description and source-code
```javascript
getRunningProcessType = function (seleniumArgs) {
  var roleArg = seleniumArgs.indexOf('-role');
  var role = (roleArg !== -1) ? seleniumArgs[roleArg + 1] : undefined;

  if (roleArg === -1) return PROCESS_TYPES.STANDALONE;
  else if (role === 'hub') return PROCESS_TYPES.GRID_HUB;
  else if (role === 'node') return PROCESS_TYPES.GRID_NODE;
  else return undefined;
}
```
- example usage
```shell
...
if (roleArg === -1) return PROCESS_TYPES.STANDALONE;
else if (role === 'hub') return PROCESS_TYPES.GRID_HUB;
else if (role === 'node') return PROCESS_TYPES.GRID_NODE;
else return undefined;
}

exports.getSeleniumStatusUrl = function(seleniumArgs) {
var processType = this.getRunningProcessType(seleniumArgs);
var portArg = seleniumArgs.indexOf('-port');
var port = (portArg !== -1) ? seleniumArgs[portArg + 1] : undefined;
var hostArg = seleniumArgs.indexOf('-host');
var host = (hostArg !== -1) ? seleniumArgs[hostArg + 1] : 'localhost';

var statusURI = new URI('http://' + host);
var nodeStatusAPIPath = '/wd/hub/status';
...
```

#### <a name="apidoc.element.selenium-standalone.get_selenium_status_url.getSeleniumStatusUrl"></a>[function <span class="apidocSignatureSpan">selenium-standalone.get_selenium_status_url.</span>getSeleniumStatusUrl (seleniumArgs)](#apidoc.element.selenium-standalone.get_selenium_status_url.getSeleniumStatusUrl)
- description and source-code
```javascript
getSeleniumStatusUrl = function (seleniumArgs) {
  var processType = this.getRunningProcessType(seleniumArgs);
  var portArg = seleniumArgs.indexOf('-port');
  var port = (portArg !== -1) ? seleniumArgs[portArg + 1] : undefined;
  var hostArg = seleniumArgs.indexOf('-host');
  var host = (hostArg !== -1) ? seleniumArgs[hostArg + 1] : 'localhost';

  var statusURI = new URI('http://' + host);
  var nodeStatusAPIPath = '/wd/hub/status';
  var hubStatusAPIPath = '/grid/api/hub';

  switch (processType) {
    case PROCESS_TYPES.STANDALONE:
      statusURI.port(4444);
      statusURI.path(nodeStatusAPIPath);
      break;
    case PROCESS_TYPES.GRID_HUB:
      statusURI.port(4444);
      statusURI.path(hubStatusAPIPath);
      break;
    case PROCESS_TYPES.GRID_NODE:
      statusURI.port(5555);
      statusURI.path(nodeStatusAPIPath);
      break;
    default:
      throw 'ERROR: Trying to run selenium in an unknown way.';
  }

  // Running with a non-default port
  if (portArg !== -1) {
    statusURI.port(seleniumArgs[portArg + 1]);
  }

  return statusURI.toString();
}
```
- example usage
```shell
...
module.exports = checkStarted;

var request = require('request').defaults({json: true});
var statusUrl = require('./get-selenium-status-url.js');

function checkStarted(seleniumArgs, cb) {
var retries = 0;
var hub = statusUrl.getSeleniumStatusUrl(seleniumArgs);
// server has one minute to start
var retryInterval = 200;
var maxRetries = 60 * 1000 / retryInterval;

function hasStarted() {
  retries++;
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
