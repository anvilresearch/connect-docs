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

#### Initializing the database

In current versions of Anvil Connect, database initialization is handled by the
server at run-time. See [Run](#Run) for details.

`nv migrate` has been deprecated.

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

When Anvil Connect starts for the first time, it will check to see whether or
not the Redis server at the configured hostname and port has any data inside and
whether or not it contains data from a valid Anvil Connect instance.

If Anvil Connect detects any existing data in the database, and it is not from
an Anvil Connect instance, it will halt with the following error:

```
Redis already contains data, but it doesn't seem to be an Anvil Connect database.
If you are SURE it is, start the server with --no-db-check to skip this check.
```

If you are sure that there is no conflicting data in Redis, for example,
in the event that you may have edited Redis's data manually, start Anvil Connect
with the `--no-db-check` flag.

```bash
$ node server.js --no-db-check
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

You can see all the natively supported providers in the [providers directory](https://github.com/anvilresearch/connect/tree/master/providers) in the Anvil Connect repository on GitHub. If you want to use an provider not listed there, you can easily add support in your instance by creating a simple configuration file in a providers directory in your project repository.

#### amr claim

The `amr` claim is a part of the [OpenID Connect specification][oidcimplicit]
which stands for Authentication Methods References. It determines which methods
of authentication were used during the authentication request.

For example, if a user signs in with their e-mail and password, then one of the
values of the `amr` claim will be `pwd` for password authentication. If the user
also used two-factor authentication with a disposable token, then the `amr`
claim will be `[ 'mfa', 'pwd', 'otp' ]` for multi-factor authentication,
password authentication, and one-time password.

Anvil Connect supports taking advantage of the `amr` claim given that it is
defined for a particular provider. You can also configure your own value for the
amr claim for a particular provider in the configuration file.

We use, and recommend the use of, the [IETF amr values draft][ietfamrvalues]
as a starting point for choosing amr values.

For example, to define an `amr` value of `ad` for authenticating with Active
Directory:

```json
{
  // ...
  "providers": {
    "ActiveDirectory": {
      "amr": "ad",
      // ...
    }
  }
}
```

#### Password authentication

To enable password authentication, add a `password` property to the `providers` object with a value of `true`. When set to true, `password` _requires_ login with username/password combination for the given providers every time they sign in. If set to `false`, the user will be able to sign in without authenticating with via username/password with the provider if as they are externally logged into that provider already.

```json
{
  // ...
  "providers": {
    "password": true
  }
}
```

#### OAuth 2.0

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

#### OAuth 1.0

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
#### LDAP

Because of LDAP's flexible nature, LDAP support is provided in the form of a
provider template which can be used to handle custom LDAP attribute to OpenID
Connect claim scenarios. A basic provider that inherits this template without
modification is provided for convenience.

LDAP providers can be configured as follows:

```json
{
  // ...
  "providers": {
    "LDAP": {
      "url": "ldap://corp.example.com",
      "bindDn": "cn=admin,dc=example,dc=com",
      "bindCredentials": "pass1234",
      "searchBase": "ou=people,dc=example,dc=com",
      "searchFilter": "(cn={{username}})"
    }
  }
}
```

A [full list of configuration options][passport-ldapauth-config] is available
from the passport-ldapauth library.

By default, LDAP attributes map to OpenID Connect claims as such:

OpenID Connect claim | LDAP attribute
-------------------- | --------------
id | dn
email | mail
name | cn
given\_name | givenName
family\_name | sn
phone\_number | telephoneNumber
address.formatted | postalAddress
address.street\_address | street
address.locality | l
address.region | st
address.postal\_code | postalCode
address.country | co

To customize these mappings, simply create your own provider in the `providers`
folder of your Anvil Connect instance. For example:

```js
module.exports = function(config) {
  return {
    id: 'MyLDAP',
    name: 'Example Corp.',
    templates: [ 'LDAP' ],
    mapping: {
      id: 'uid',
      name: 'displayName'
  };
};
```


##### Active Directory

The expected configuration format for the Active Directory provider is as follows:

```json
{
  ...
  "providers": {
    "ActiveDirectory": {
      "url": "ldaps://corp.example.com",
      "domainDn": "dc=example,dc=com",
      "tlsOptions": {
        "ca": "/path/to/the/self/signed/ca-cert.cer"
      }
    }
  }
}
```

Anvil Connect also provides the `ActiveDirectory` provider template in the event that you may be working with several domains at a time or if you would like to customize certain aspects of the AD provider, such as the name.

Here's an example provider that uses the template:

```javascript
module.exports = function(config) {
  return {
    id: 'examplecorpad',
    name: 'Example Corporation',
    templates: [ 'ActiveDirectory' ]
  };
};
```

To configure this provider, you would use `examplecorpad` in place of `ActiveDirectory` in your configuration.

```json
{
  ...
  "providers": {
    "examplecorpad": { ... }
  }
}
```


##### Groups

The LDAP provider will synchronize the user's role membership in Anvil Connect with their group membership in the domain. In order to take advantage of this feature, each LDAP group for which you wish to enable synchronization must have a respective role in Connect named after the fully-qualified distinguished name (FQDN) of the group in the directory.

For example, if there exists a group in the directory service with FQDN `CN=Group 1,OU=Groups,DC=example,DC=com`, that group will only influence the user's role membership if there also exists a role in Connect named `CN=Group 1,OU=Groups,DC=example,DC=com`. You can create this role using the `nv add role` command:

```bash
nv add role '{ "name": "CN=Group 1,OU=Groups,DC=example,DC=com" }'
```


### Configuring the mailer

Anvil Connect uses [nodemailer](https://github.com/andris9/Nodemailer) for sending emails for the purposes of email verification, and other functionality which is to come.

The configuration file (e.g. development.json, production.json) must have a mailer object on the top level. The options for this configuration object are identical as that of [nodemailer transports](https://github.com/andris9/Nodemailer#use-the-default-smtp-transport), with the exception of an additionally required `from` property. The `from` property defines what e-mail address and name Anvil Connect will use in sending e-mails. It must be in the form `Name <no-reply@example.com>`.

```json
{
  "mailer": {
    "from": "Hello World <test@gmail.com>",
    "view_engine": "hogan",
    "service": "Gmail",
    "auth": {
      "user": "test@gmail.com",
      "pass": "test"
    }
  },
}
```

### Configuring e-mail verification

Anvil Connect supports verifying user e-mail addresses to ensure that the user has ownership/control over their registered address. By default, configuring the mailer will enable email verification server-wide, but not require it.

When a user signs in or signs up using e-mail/password based authentication, their e-mail address is unverified at first. If the user signs in or signs up using a third-party provider, then Connect will inherit the `email_verified` claim if available. Otherwise, the user's e-mail address stays unverified.

#### Flow

1. **Email verification enabled, but not required**
   The user is able to sign up for an account with Anvil Connect, and sign in to clients as they normally would, without interruption. However, when the user signs up for the first time, they receive an email asking them to verify their email address.
2. **Email verification enabled and required**
   When the user signs up for the first time with an unverified e-mail address, they are redirected to a page that prompts them to check their e-mail for a verification e-mail message. They are also given the option on that page to resend the e-mail in the event that it hasn't made its way through. Until the user verifies their e-mail, they are unable to authenticate with any client.

#### Configuring verification for the entire server

```json
{
  "emailVerification": {
    "enable": true,
    "require": false
  },
  ...
}
```

#### Configuring verification for specific providers

```json
{
  "providers": {
    "password": {
      "emailVerification": {
        "enable": true,
        "require": false
      }
    }
  }
  ...
}
```


### Redis

Anvil Connect requires access to a Redis database and uses the default host and port for a local instance. To use a remote Redis server, provide host, port, and password parameters under the "redis" object in your config.

```json
{
  // ...
  "redis": {
    "host": "HOST",
    "port": "PORT",
    "password": "PASSWORD"
  }
}
```

Configuration options for Redis are identical to that of the [supported options for ioredis][ioredis-options].

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

You can change the look and feel of Anvil Connect by editing the contents of the `views` directory in your project. There are four templates included in your views directory: `authorize`, `signin`, `signup`, and `session`. Connect works with any templating language supported by [consolidate.js](https://github.com/tj/consolidate.js). The `jade` templating language is used by default. You can configure your server to use a different templating language with the `view_engine` setting in your config files.

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

### E-mail templates

E-mail templates are stored in the emails directory in the Connect root directory. The file extension matches the view engine to be used. The view engine can be configured by modifying the `view_engine` property on the `mailer` configuration object. Just as with views, any view engine supported by [consolidate.js](https://github.com/tj/consolidate.js) is supported by this property. By default, it is set to use [Hogan](https://github.com/twitter/hogan.js).

#### E-mail verification

The template for e-mail verification messages is `verifyEmail`. The template is provided with the provider name (`providerName`), verification URL (`verifyURL`), and user e-mail (`email`).

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

[oidcimplicit]: http://openid.net/specs/openid-connect-implicit-1_0.html
[ietfamrvalues]: http://tools.ietf.org/html/draft-jones-oauth-amr-values-00
[passport-ldapauth-config]: https://github.com/vesse/node-ldapauth-fork/blob/master/lib/ldapauth.js#L22-L94
[ioredis-options]: https://github.com/luin/ioredis/blob/master/API.md#new-redisport-host-options
