## Protocols

Out of the box, Anvil Connect supports authenticating users with local
passwords, OAuth 1.0, OAuth 2.0, OpenID, LDAP, and Active Directory. We call
these authentication methods "protocols".

For each supported protocol, we implement a module that supports any compliant
provider. This way, there's no need to write OAuth 2.0 code to support a new
OAuth 2.0-based service. Within Anvil Connect, [providers](providers/) and
protocols work together to separate the differences between authentication
methods from the parts that remain the same.

In addition to what we ship with Anvil Connect, it only takes a small amount of
code in your project to authenticate using virtually any other method. You can
customize your Connect instance to use additional providers and protocols by
creating special directories in your project and adding some JavaScript.

### Adding protocols

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

