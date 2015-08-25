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
- [LDAP](LDAP.md)

