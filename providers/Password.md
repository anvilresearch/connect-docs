## Password authentication

To enable password authentication, add a `password` property to the `providers`
object with a value of `true`. When set to true, `password` _requires_ login
with username/password combination for the given providers every time they sign
in. If set to `false`, the user will be able to sign in without authenticating
with via username/password with the provider if as they are externally logged
into that provider already.

```json
{
  // ...
  "providers": {
    "password": true
  }
}
```
