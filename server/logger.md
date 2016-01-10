## Logger

Anvil Connect uses [bunyan](https://github.com/trentm/node-bunyan) for logging.

### JSON Example

The default `stdout` and `file` streams can be enabled or disabled by editing your JSON configuration file.

```json
{
  // ...
  "logger": {
    "stdout": true,
    "file": true
  }
}
```

### JavaScript Example

You can also create more sophisticated logging schemes by with a module called `logger.js` in the same directory of your project as `server.js`.

```javascript
module.exports = {
  name: 'anvilconnect',
  streams: [
    {
      level: 'info',
      stream: process.stdout            // log INFO and above to stdout
    },
    {
      level: 'error',
      path: '/var/tmp/myapp-error.log'  // log ERROR and above to a file
    }
  ]
}
```



