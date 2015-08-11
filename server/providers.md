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

