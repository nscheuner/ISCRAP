# iScrap 

Subtitle scraper and publisher - Academic projet - HEIG-VD 2016",

Out of the box it supports the following features:

* Amazon ec2 instance creation and handling with 24h ip blacklist 
* Download subtitle over SSH and increment error count if redirection
* MovieCollection aggregator
* IMDb and opensubtitles.org scrapper, gets youtube video id for video trailer
* Databse seed from opensubtitles.org list
* Create and Update methode for wordpress post
* Handlebar template for wordpress post
* Publish subtitles according to a website scope stored in databse
* Comprehensive logs
* Error handeling

NOTE: from log4js 0.5 onwards you'll need to explicitly enable replacement of node's console.log functions. Do this either by calling `log4js.replaceConsole()` or configuring with an object or json file like this:

```javascript
{
  appenders: [
    { type: "console" }
  ],
  replaceConsole: true
}
```

## installation

npm install log4js


## usage

Minimalist version:
```javascript
var log4js = require('log4js');
var logger = log4js.getLogger();
logger.debug("Some debug messages");
```
By default, log4js outputs to stdout with the coloured layout (thanks to [masylum](http://github.com/masylum)), so for the above you would see:
```bash
[2010-01-17 11:43:37.987] [DEBUG] [default] - Some debug messages
```
See example.js for a full example, but here's a snippet (also in fromreadme.js):
```javascript
var log4js = require('log4js'); 
//console log is loaded by default, so you won't normally need to do this
//log4js.loadAppender('console');
log4js.loadAppender('file');
//log4js.addAppender(log4js.appenders.console());
log4js.addAppender(log4js.appenders.file('logs/cheese.log'), 'cheese');

var logger = log4js.getLogger('cheese');
logger.setLevel('ERROR');

logger.trace('Entering cheese testing');
logger.debug('Got cheese.');
logger.info('Cheese is Gouda.');
logger.warn('Cheese is quite smelly.');
logger.error('Cheese is too ripe!');
logger.fatal('Cheese was breeding ground for listeria.');
```
Output:
```bash
[2010-01-17 11:43:37.987] [ERROR] cheese - Cheese is too ripe!
[2010-01-17 11:43:37.990] [FATAL] cheese - Cheese was breeding ground for listeria.
```    
The first 5 lines of the code above could also be written as:
```javascript
var log4js = require('log4js');
log4js.configure({
  appenders: [
    { type: 'console' },
    { type: 'file', filename: 'logs/cheese.log', category: 'cheese' }
  ]
});
```

## configuration

You can configure the appenders and log levels manually (as above), or provide a
configuration file (`log4js.configure('path/to/file.json')`), or a configuration object. The 
configuration file location may also be specified via the environment variable 
LOG4JS_CONFIG (`export LOG4JS_CONFIG=path/to/file.json`). 
An example file can be found in `test/log4js.json`. An example config file with log rolling is in `test/with-log-rolling.json`.
You can configure log4js to check for configuration file changes at regular intervals, and if changed, reload. This allows changes to logging levels to occur without restarting the application.

To turn it on and specify a period:

```javascript
log4js.configure('file.json', { reloadSecs: 300 });
```
For FileAppender you can also pass the path to the log directory as an option where all your log files would be stored.

```javascript
log4js.configure('my_log4js_configuration.json', { cwd: '/absolute/path/to/log/dir' });
```
If you have already defined an absolute path for one of the FileAppenders in the configuration file, you could add a "absolute": true to the particular FileAppender to override the cwd option passed. Here is an example configuration file:

#### my_log4js_configuration.json ####
```json
{
  "appenders": [
    {
      "type": "file",
      "filename": "relative/path/to/log_file.log",
      "maxLogSize": 20480,
      "backups": 3,
      "category": "relative-logger"
    },
    {
      "type": "file",
      "absolute": true,
      "filename": "/absolute/path/to/log_file.log",
      "maxLogSize": 20480,
      "backups": 10,
      "category": "absolute-logger"          
    }
  ]
}
```    
Documentation for most of the core appenders can be found on the [wiki](https://github.com/nomiddlename/log4js-node/wiki/Appenders), otherwise take a look at the tests and the examples.

## Documentation
See the [wiki](https://github.com/nomiddlename/log4js-node/wiki). Improve the [wiki](https://github.com/nomiddlename/log4js-node/wiki), please.

There's also [an example application](https://github.com/nomiddlename/log4js-example).

## Contributing
Contributions welcome, but take a look at the [rules](https://github.com/nomiddlename/log4js-node/wiki/Contributing) first.

## License

The original log4js was distributed under the Apache 2.0 License, and so is this. I've tried to
keep the original copyright and author credits in place, except in sections that I have rewritten
extensively.
