## SAML 2.0

Anvil Connect uses [passport-saml][passport-saml] to act as a SAML 2.0 Service Provider. 
You can [configure][passport-saml-configure] any options supported by that package.

```json
{
  // ...
  "providers": {
    "SAML2": {
      // single sign-on endpoint
      "entryPoint": "https://HOST:PORT/PATH",
    }    
  }
}
```

[passport-saml]: https://github.com/bergie/passport-saml
[passport-saml-configure]: https://github.com/bergie/passport-saml#configure-strategy
