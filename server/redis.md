### Redis

Anvil Connect requires access to a Redis database and uses the default host and port for a local instance. To use a remote Redis server, provide url and auth parameters under the "redis" object in your config.

```json
{
  // ...
  "redis": {
    "url": "redis://HOST:PORT",
    "auth": "PASSWORD"
  }
}
```

You can also provide a `db` setting (integer) if you want to use a different Redis database. By default, Redis is configured to support 16 databases (0 - 15). This can be configured in the `redis.conf` file for your Redis installation.

```json
{
  // ...
  "redis": {
    "db": 3
  }
}
```

