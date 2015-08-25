## OAuth 2.0

Most OAuth 2.0 providers only require a `client_id` and `client_secret`. You can
obtain these by registering your app with the respective provider.

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

OAuth 2.0 supports a `scope` authorization parameter, and some providers use it
to restricted access to specific resources. You can set scope for a provider
using the `scope` property with an array of strings. See provider API
documentation for specifics.

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

### Creating a new OAuth 2.0 provider

To integrate with a provider we don't support out of the box, you can create 
your own provider definition in a custom `connect/providers` directory in your
project. You can use [officially supported provider definitions][supported-providers] 
as examples.

[supported-providers]: https://github.com/anvilresearch/connect/tree/master/providers

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

