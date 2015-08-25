## Providers

A "provider" is a specific service providing authentication by means of a given
protocol. For example, Facebook, GitHub, and Dropbox are OAuth 2.0 providers.
They implement OAuth 2.0, and that's the protocol we use to interface with them
in the auth server.

### Overview

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

#### id property

The `id` property is used by the server to match a provider-specific
authorization request to this provider metadata. A good heuristic is to name
it the same as the filename (without the `.js` extension).

#### protocol property

The `protocol` property corresponds to the file name (sans extension) of the
protocol module you wish to use. Built in protocols you may wish to add a
provider for include `OAuth`, `OAuth2`, `OpenID`, `LDAP`, and
`ActiveDirectory`. If you add a [custom protocol](#?), use the basename of the
file for this value.

#### mapping property

The `mapping` property determines how user info from the provider gets mapped
into the OpenID Connect claims stored for each user inside Anvil Connect. On
the left are Anvil Connect user properties and on the right are property names
from the provider. You can reference nested properties using dot notation.
Given an object `{ info: { emails: { home: "foo@example.com" } } }`, your
mapping would contain `email: 'info.emails.home'`. The value side of a mapping
can also be a function, where the argument is the entire user info object from
the provider.

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

#### refresh_userinfo

When a user registers with an Anvil Connect instance using an external provider
for the first time, the server will attempt to map any userinfo available from 
that provider to OpenID Connect standard claims before storing the new user.

By default this only happens the first time a user logs in. Anvil Connect can 
be configured to refresh and re-map this userinfo each time a user authenticates
using the `refresh_userinfo` setting. This behavior can be specified globally 
for the entire server or individually for each provider.

```json
{
  // global setting
  "refresh_userinfo": true,
  
  // provider specific setting
  "providers": {
    "github": {
      "refresh_userinfo": false  
    },
    "ldap": {
      "refresh_userinfo": true
    }
  }
  
}

```


#### Contributing commonly used providers

Contributing new OAuth and OAuth 2.0 providers is one of the easiest ways to
get involved with the open source project. While many custom providers may not
be suitable for inclusion in the core of Anvil Connect, we greatly appreciate
pull requests for those that are.

### Creating and configuring providers

Instructions for adding different types of providers are given throughout
this folder.

- [Password](Password.md)
- [OAuth 2.0](OAuth2.md)
- [OAuth](OAuth.md)
- [LDAP and Active Directory](LDAP.md)

