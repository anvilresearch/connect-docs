# Server

## Get Started

### Requirements

Anvil Connect is built with the latest versions of [Node.js](https://nodejs.org/) (0.12.x) and [Redis](http://redis.io/) (3.0.x). You'll need these installed on your system before you can run the server.




### Install

Install Anvil Connect globally with npm to install the CLI.

```bash
$ npm install -g anvil-connect
```

### Initialize








#### Generate deployment

Once you have installed the CLI, make a new directory and initialize your project.

```bash
$ mkdir path/to/project && cd $_
$ nv init
```

This will generate a file tree that looks something like this:

```bash
├── .bowerrc
├── .git
├── .gitignore
├── Dockerfile
├── bower.json
├── config
│   ├── development.json
│   ├── keys
│   │   ├── private.pem
│   │   └── public.pem
│   └── production.json
├── package.json
├── public
│   ├── images
│   │   └── anvil.svg
│   ├── javascript
│   │   └── session.js
│   └── stylesheets
│       └── app.css
├── server.js
└── views
    ├── authorize.jade
    ├── session.jade
    ├── signin.jade
    └── signup.jade
```

Anvil Connect aims to be easily customizable. Using a deployment repository allows you to serve your own static assets, customize views (HTML templates), manage dependencies and keep your configuration under version control. It also makes upgrading Anvil Connect as simple as changing the version number in `package.json`.

#### Install Dependencies

Now you can install npm and bower dependencies.

```bash
$ npm install
$ bower install
```

#### Initialize the database

If you're using a fresh Redis installation running on `localhost` and you're ok with using the default (preferably empty) database, there's nothing to configure. If you're using a remote Redis instance, your instance requires a password, or you want to use a database other than `0`, [edit the config file](#redis) for the environment you're preparing (development or production).

Then, to initialize your development database, run:

```bash
$ nv migrate
```

To initialize a production database, run:

```bash
$ NODE_ENV=production nv migrate
```

This will create default clients, roles, scopes and permissions necessary to operate the authorization server.









### Run

#### Environments
#### Commands

Run the authorization server in `development` mode:

```bash
# Any of the following are equivalent
$ nv serve
$ node server.js
$ npm start
```

To run the server in production, set `NODE_ENV`:

```bash
# Any of the following are equivalent
$ nv serve --production
$ node server.js -e production
$ NODE_ENV=production node server.js
```




## Configure

### JSON files

Anvil Connect loads it's configuration from a JSON file in the `config` directory of the current working directory for the process. File names must match the `NODE_ENV` value. If `NODE_ENV` is not set, `config/development.json` will be loaded.

### Key pairs

If you generated a deployment repository with `nv init`, a new RSA key pair will be generated for you in `config/keys`. This pair of files is required for signing and verifying tokens. We recommend using the generated files. If you want to provide your own, you can obtain them using OpenSSL.

```
$ cd PROJECT_ROOT
$ mkdir -p config/keys
$ openssl genrsa -out config/keys/private.pem 2048
$ openssl rsa -pubout -in config/keys/private.pem -out config/keys/public.pem
```



### Server Settings

##### issuer

Fully qualified base uri of the authorization server; e.g., <code>https://accounts.anvil.io</code>

##### port

An integer value representing the port the server will be bound to, unless a <code>PORT</code> environment variable is provided. Defaults to <code>3000</code>.

##### cookie_secret

A string used for signing cookies. When you initialize a project, this value is generated for each of your environments. Treat it as confidential and always use separate values for each project and environment.

##### session_secret

A string used for signing session ID cookies. When you initialize a project, this value is generated for each of your environments. Treat it as confidential and always use separate values for each project and environment.

##### client_registration

Anvil Connect can be configured for three types of client registration: `dynamic`, `token`, or `scoped`, each being more restrictive than the previous option. The default `client_registration` type is `scoped`. For more details, see the section titled [Client Registration](#client-registration-1).

##### trusted_registration_scope

Trusted clients require additional scope to register. This can be configured with the `trusted_registration_scope` setting, which defaults to `realm`.


### Providers

The providers setting is an object containing settings for various authentication methods.

```json
{
  // ...
  "providers": { ... }
}
```

To enable local password authentication, add a `password` property to the `providers` object with a value of `true`.

```json
{
  // ...
  "providers": {
    "password": true
  }
}
```

Most OAuth 2.0 providers only require a `client_id` and `client_secret`. You can obtain these by registering your app with the respective provider.

```json
{
  // ...
  "providers": {
    "facebook": {
      "client_id": "App ID",
      "client_secret": "App Secret"
    }
  }
}
```

OAuth 2.0 supports a `scope` authorization parameter, and some providers use it to restricted access to specific resources. You can set scope for a provider using the `scope` property with an array of strings. See provider API documentation for specifics.

```json
{
  //...
  "providers": {
    "google": {
      "client_id": "Client ID",
      "client_secret": "Client secret",
      "scope": [
        "https://www.googleapis.com/auth/userinfo.profile",
        "https://www.googleapis.com/auth/userinfo.email"
      ]
    },
    "linkedin": {
      "client_id": "Client ID",
      "client_secret": "Client Secret",
      "scope": [
        "r_basicprofile",
        "r_fullprofile",
        "r_emailaddress",
        "r_network",
        "r_contactinfo"
      ]
    }
  }
}
```

OAuth 1.0 providers require `oauth_consumer_key` and `oauth_consumer_secret`.


```json
{
  //...
  "providers": {
    "twitter": {
      "oauth_consumer_key": "Consumer Key (API Key)",
      "oauth_consumer_secret": "Consumer Secret"
    }
  }
}
```

You can see all the natively supported providers in the [providers directory](https://github.com/anvilresearch/connect/tree/master/providers) in the Anvil Connect repository on GitHub. If you want to use an OAuth provider not listed there, you can easily add support in your instance by creating a simple configuration file in a providers directory in your project repository.




### Redis

Anvil Connect requires access to a Redis server and uses the default host and port for a local instance. To use a remote Redis server, provide url and auth parameters.

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

### Logger

Anvil Connect uses [bucker](https://github.com/nlf/bucker) for logging. Any valid configuration parameters for bucker can be included in the "logger" parameter. For example:

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


### OpenID Metadata

[OpenID Provider Metadata](http://openid.net/specs/openid-connect-discovery-1_0.html#ProviderMetadata) default values can be overridden by defining them in the configuration file. Don't change these unless you know what you're doing.








## Customize

<!--
### Views
### Hooks
### Providers
### Protocols
-->

## Deploy

<!--
### SSL
### Nginx
### Docker
### Joyent
### Digital Ocean
### AWS
### Heroku
### Modulus
-->
