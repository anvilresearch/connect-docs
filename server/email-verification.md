### Configuring e-mail verification

Anvil Connect supports verifying user e-mail addresses to ensure that the user
has ownership/control over their registered address. By default, configuring the
mailer will enable email verification server-wide, but not require it.

When a user signs in or signs up using e-mail/password based authentication,
their e-mail address is unverified at first. If the user signs in or signs up
using a third-party provider, then Connect will inherit the `email_verified`
claim if available. Otherwise, the user's e-mail address stays unverified.

#### Flow

1. **Email verification enabled, but not required**  
   The user is able to sign up for an account with Anvil Connect, and sign in
   to clients as they normally would, without interruption. However, when the
   user signs up for the first time, they receive an email asking them to verify
   their email address.
2. **Email verification enabled and required**  
   When the user signs up for the first time with an unverified e-mail address,
   they are redirected to a page that prompts them to check their e-mail for a
   verification e-mail message. They are also given the option on that page to
   resend the e-mail in the event that it hasn't made its way through. Until the
   user verifies their e-mail, they are unable to authenticate with any client.

#### Configuring verification for the entire server

```json
{
  "emailVerification": {
    "enable": true,
    "require": false
  },
  ...
}
```

#### Configuring verification for specific providers

```json
{
  "providers": {
    "password": {
      "emailVerification": {
        "enable": true,
        "require": false
      }
    }
  }
  ...
}
```

