## LDAP

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


### Active Directory

The expected configuration format for the Active Directory provider is as
follows:

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

Anvil Connect also provides the `ActiveDirectory` provider template in the event
that you may be working with several domains at a time or if you would like to
customize certain aspects of the AD provider, such as the name.

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

To configure this provider, you would use `examplecorpad` in place of
`ActiveDirectory` in your configuration.

```json
{
  ...
  "providers": {
    "examplecorpad": { ... }
  }
}
```


### Groups

The LDAP provider will synchronize the user's role membership in Anvil Connect
with their group membership in the domain. In order to take advantage of this
feature, each LDAP group for which you wish to enable synchronization must have
a respective role in Connect named after the fully-qualified distinguished name
(FQDN) of the group in the directory.

For example, if there exists a group in the directory service with FQDN
`CN=Group 1,OU=Groups,DC=example,DC=com`, that group will only influence the
user's role membership if there also exists a role in Connect named
`CN=Group 1,OU=Groups,DC=example,DC=com`. You can create this role using the
`nv add role` command:

```bash
nv add role '{ "name": "CN=Group 1,OU=Groups,DC=example,DC=com" }'
```

