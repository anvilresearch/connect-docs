## Password authentication

Local password authentication can be configured easily by adding a password
object to the providers section of your configuration files.

Anvil Connect uses the mellt package to test password strength. You can
configure the minimum number of days required to crack a password with the
`daysToCrack` setting, which defaults to 14.

```json
{
  // ...
  "providers": {
    "password": {
      "daysToCrack": 21
    }
  }
}
```
