## User Authentication

Out of the box, Anvil Connect supports authenticating users with local
passwords, OAuth 1.0, OAuth 2.0, OpenID, LDAP, and ActiveDirectory. We call
these authentication methods "protocols".

A "provider" is a specific service providing authentication by means of a given
protocol. For example, Facebook, GitHub, and Dropbox are OAuth 2.0 providers.
They implement OAuth 2.0, and that's the protocol we use to interface with them
in the auth server.

For each supported protocol, we implement a module that supports any compliant
provider. This way, there's no need to write OAuth 2.0 code to support a new
OAuth 2.0-based service. Within Anvil Connect, providers and protocols work
together to separate the differences between authentication methods from the
parts that remain the same.

In addition to what we ship with Anvil Connect, it only takes a small amount of
code in your project to authenticate using virtually any other method. You can
customize your Connect instance to use additional providers and protocols by
creating special directories in your project and adding some JavaScript.



### Adding Providers

To add a new provider, create a `connect/providers` directory in your Anvil
Connect project and add a file named for the provider. The filename (not
including the .js extension) must be a valid JSON key since you'll be using it
in your configuration file. Let's say we want to add an OAuth 2.0 provider for
GitHub (although this provider is already supported). We'll create a file
called `connect/providers/github.js`. It should export a function that takes a
`config` argument which contains all settings for the server.

```
/**
 * GitHub
 */

module.exports = function (config) {
  return {
    id:                   'github',
    name:                 'GitHub',
    protocol:             'OAuth2',
    // ...
    // protocol specific properties
    // ...
    mapping: {
      id:                 'id',
      email:              'email',
      name:               'name',
      website:            'blog',
      preferredUsername:  'login',
      profile:            'html_url',
      picture:            'avatar_url',
    }
  };
};
```

When invoked, this function returns a simple JavaScript object containing all
the parameters needed by the corresponding protocol.

#### id Property

The `id` property is used by the server to match a provider-specific
authorization request to this provider metadata. A good heuristic is to name
it the same as the filename (without the `.js` extension).

#### Protocol Property

The `protocol` property corresponds to the file name (sans extension) of the
protocol module you wish to use. Built in protocols you may wish to add a
provider for include `OAuth`, `OAuth2`, `OpenID`, `LDAP`, and
`ActiveDirectory`. If you add a [custom protocol](#?), use the basename of the
file for this value.

#### Mapping Property

The `mapping` property determines how user info from the provider gets mapped
into the OpenID Connect claims stored for each user inside Anvil Connect. On
the left are Anvil Connect user properties and on the right are property names
from the provider. You can reference nested properties using dot notation.
Given an object `{ info: { emails: { home: "foo@example.com" } } }`, your
mapping would contain `email: 'info.emails.home'`. The value side of a mapping
can also be a function, where the argument is the entire user info object from
the provider.


#### OAuth 2.0 Providers


```
/**
 * GitHub
 */

module.exports = function (config) {
  return {
    id:                   'github',
    name:                 'GitHub',
    protocol:             'OAuth2',
    url:                  'https://github.com',
    redirect_uri:          config.issuer + '/connect/github/callback',
    endpoints: {
      authorize: {
        url:              'https://github.com/login/oauth/authorize',
        method:           'POST',
      },
      token: {
        url:              'https://github.com/login/oauth/access_token',
        method:           'POST',
        auth:             'client_secret_post'
      },
      user: {
        url:              'https://api.github.com/user',
        method:           'GET',
        auth: {
          header:         'Authorization',
          scheme:         'Bearer'
        }
      }
    },
    separator:            ',',
    mapping: {
      id:                 'id',
      email:              'email',
      name:               'name',
      website:            'blog',
      preferredUsername:  'login',
      profile:            'html_url',
      picture:            'avatar_url',
    }
  };
};
```


#### OAuth 1.0 Providers

#### LDAP Providers

#### Active Directory Providers

#### OpenID Providers


#### Contributing commonly used providers

Contributing new OAuth and OAuth 2.0 providers is one of the easiest ways to
get involved with the open source project. While many custom providers may not
be suitable for inclusion in the core of Anvil Connect, we really appreciate
pull requests for those that are.




### Adding Protocols

To add a new protocol, create a `connect/protocols` directory in your Anvil
Connect project and add a JavaScript file named for the protocol. The filename
should be a camel cased string.

This new module should export a Passport strategy that has two extra functions
appended to it: `verifier` and `initialize`. You can use any virtually existing
Passport strategy as a starting point, or write your own from scratch.


```javascript
/**
 * Dependencies
 */

var Strategy = require('passport-ANYTHING').Strategy;


/**
 * Verifier
 */

function verifier (req, [?..,] done) {
  // ...
};

Strategy.verifier = verifier;


/**
 * Initialize
 */

function initialize (provider, configuration) {
  return new Strategy([...], verifier);
}

Strategy.initialize = initialize;


/**
 * Export
 */

module.exports = strategy;
```

The verifier function is the callback that get's invoked by Passport once
authentication has completed.

#### Verifier

#### Initialize
