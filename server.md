# Server


## Get Started

### Requirements

Anvil Connect is built with the latest versions of [Node.js](https://nodejs.org/) (0.12.x) and [Redis](http://redis.io/) (3.0.x). You'll need these installed on your system before you can run the server. In light of the Node shakeup that resulted in the Node Foundation's creation, it should be noted that Connect will be updating as Node continues to evolve.


### Install
To install Connect, run the npm install command:

```bash
$ npm install -g anvil-connect
```

> **Note:** Windows support hasn't been tested, but you must have Python 2.7 and Visual Studio, which are required for compiling parts of Connect. If you would like to help contribute to Windows support, let us [know](https://github.com/anvilresearch/connect/issues), and we'll be able to work together.

After npm is finished installing Connect, you will have access to the Connect command line tool to start working on integrating Connect into your project.


### Initialize

#### Generate deployment

Once you have installed the CLI, make a new directory and initialize your project.

```bash
$ mkdir path/to/project && cd path/to/project
$ nv init
```

<!-- Is $_ a real command? 99.99% sure it won't work on Windows.-->

This will generate a file tree in the **current** directory:

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

`nv init` will also write to the console the next steps needed to finish Connect setup once the file tree has been created.

Anvil Connect aims to be easily customizable. Using a deployment repository allows you to serve your own static assets, customize views (HTML templates), manage dependencies and keep your configuration under version control. It also makes upgrading Anvil Connect as simple as changing the version number in `package.json`.

#### Install Dependencies

Now you can install npm and bower dependencies, if you want to run Connect locally.

```bash
$ npm install
$ bower install
```


#### Initialize the database

If you're using a fresh Redis installation running on `localhost` and you're ok with using the default (preferably empty) database, because there's no need to configure Redis to be used with Connect. If you're using a remote Redis instance, your instance requires a password, or you want to use a database other than `0`, [edit the config file](#redis) for the environment you're preparing (development or production).

Then, to initialize your development database, run:

```bash
// development
$ nv migrate
```

To initialize a production database, run:

```bash
// production
$ NODE_ENV=production nv migrate
```

This will create default clients, roles, scopes and permissions necessary to operate the authorization server.


### Run

#### Environments

#### Commands
There are two environments to run the Connect server in, `development`, and `production`. The development server is for local testing, setup, and development on Connect itself. The production environment should be used when deployed to a live environment.

To run the authorization server in `development` mode, you can run the server by simply starting it:

```bash
# The following are equivalent, any of them will start the development server
$ nv serve
$ node server.js
$ npm start
```

To run the server in production, set `NODE_ENV` to `production`:

```bash
# The following are equivalent, any of them will start the production server
$ nv serve --production
$ node server.js -e production
$ NODE_ENV=production node server.js
```


## Configure

### JSON files

Anvil Connect loads its configuration from a JSON file in the `config` directory of the current working directory for the process. File names must match the `NODE_ENV` value. If `NODE_ENV` is not set, `config/development.json` will be loaded.

### Key pairs

If you generated a deployment repository with `nv init`, a new RSA key pair will be generated for you in `config/keys`. This pair of files is required for signing and verifying tokens. We recommend using the generated files. We have set up key generation to lower the barrier to entry, as it is a tedious, precise process to do by hand.

If you want or need to provide your own RSA key pair, you can obtain it using OpenSSL and import them to the proper location, `config/keys/private.pem` for the private key and `config/keys/public.pem` for the public key.

```
$ cd PROJECT_ROOT
$ mkdir -p config/keys
$ openssl genrsa -out config/keys/private.pem 2048
$ openssl rsa -pubout -in config/keys/private.pem -out config/keys/public.pem
```

### Server Settings

##### issuer

**Type:** string

**Use:** URI used to identify issuer of authentication

**Description:** Fully qualified base URI of the authorization server; e.g., <code>https://accounts.anvil.io</code>


##### port

**Type:** integer

**Use:** port # the Connect server is run under

**Description:** An integer value representing the port the server will be bound to, unless a <code>PORT</code> environment variable is provided. Defaults to <code>3000</code>.

##### cookie_secret

**Type:** string

**Use:** signing cookies  <!-- under what scope? -->

**Description:** A string used for signing cookies. When you initialize a project, this value is generated for each of your environments. Treat it as confidential and always use separate values for each project and environment.

##### session_secret

**Type:** string

**Use:** signing session cookies

**Description:** A string used for signing session ID cookies. When you initialize a project, this value is generated for each of your environments. Treat it as confidential and always use separate values for each project and environment.

##### client_registration

**Type:** string - options: `dynamic`, `token`, or `scoped`

**Use:** Assigning the type of client registration Connect uses

**Description:** Anvil Connect can be configured for three types of client registration: `dynamic`, `token`, or `scoped`, each being more restrictive than the previous option. The default `client_registration` type is `scoped`. For more details, see the section titled [Client Registration](#client-registration-1).

##### trusted_registration_scope

**Type:** string - options: `realm`, Connect operator's own registered scope(s)

**Use:** signing session cookies

**Description:** `trusted_registration_scope` signals if a client is trusted or not - trusted clients require additional scope to register. It defaults to `realm`.


### Providers

The providers setting is an object containing settings for various authentication methods.

```json
{
  // ...
  "providers": { ... }
}
```

To enable password authentication, add a `password` property to the `providers` object with a value of `true`. When set to true, `password` _requires_ login with username/password combination for the given providers every time they sign in. If set to `false`, the user will be able to sign in without authenticating with via username/password with the provider if as they are externally logged into that provider already.

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

### Logger

Anvil Connect uses [bucker](https://github.com/nlf/bucker) for logging. Any valid configuration parameters for bucker can be included in the "logger" parameter in your config file. For example:

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

### Views

You can change the look and feel of Anvil Connect by editing the contents of the `views` directory in your project. There are four templates included in your views directory: `authorize`, `signin`, `signup`, and `session`. Connect uses the `jade` templating language by default. You can configure your server to use a different templating language with the `view_engine` setting in your config files.

#### Authorize

The `authorize` view is the part of an OAuth 2.0 authorization flow that enables users to provide consent to an application to access some of their data. It is rendered when users authorize a third-party app. It is not displayed when authenticating `trusted` clients.

#### Signin

The `signin` view displays all configured user authentication options.

#### Signup

The `signup` view displays a signup form for local password authentication.

#### Session

This view is not intended to be visible to users. It gets loaded into a hidden iframe for clients that implement Single Sign-On using OpenID Connect Sessions. If you change it, be sure to leave the script references intact or this feature will not work correctly.



#### Static Assets

You can also add your own static assets to be served from the `public` directory at the root of your project.

<!--
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
