## Bunyan: A logging tool for node.js

## Motivation

We are all familiar with using **console.log** to log out information for debugging purposes. However, as the application grows, **console.log** gets littered everywhere. Also, formatting messaging is done manually.

## Introducing bunyan

A nice little logging tool that comes with time stamp, process ID and machine name formatting out of the box.  
Check out [bunyan](https://github.com/trentm/node-bunyan) here.

## Using bunyan

1. **Install the package**

`npm install bunyan`

**2. Create a logger instance**

```javascript
const log = bunyan.createLogger({
    name: 'my-logger',
    serializers: bunyan.stdSerializers
});
module.exports = log;
```

**3. Use the logging methods**

There are several methods for logging such as _info_, _warn_, _error_, _debug_.

```javascript
const logger = require(path/to/logger');
const port = process.env.PORT || 3000;
logger.info(`Express is running on port ${port}`);
```

**4. View output**

`[2018-10-29T10:04:08.045Z] INFO: my-logger/20194 on : Express is running on port 3000`

## Optional: Pretty print

Bunyan has a useful tool for pretty printing the logs into a more human readable format via CLI:

`node index.js | ./node_modules/.bin/bunyan`

## Closing

Bunyan is a nice tool that comes with great formatting for logging in production. It is easy to get started with, however customising its behaviour is not easy as the documentation isn't well organised to facilitate this.