## Logger

Anvil Connect uses [bucker](https://github.com/nlf/bucker) for logging. Any
valid configuration parameters for bucker can be included in the "logger"
parameter in your config file. For example:

```json
{
  // ...
  "logger": {
    "console": {
      "color": false
    },
    "syslog": {
      "host": "localhost",
      "port": 514,
      "facility": 18
    }
  }
}
```

