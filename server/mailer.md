## Configuring the mailer

Anvil Connect uses [nodemailer](https://github.com/andris9/Nodemailer) for
sending emails for the purposes of email verification, and other functionality
which is to come.

The configuration file (e.g. development.json, production.json) must have a
mailer object on the top level. The options for this configuration object are
identical as that of [nodemailer transports](https://github.com/andris9/Nodemailer#use-the-default-smtp-transport),
with the exception of an additionally required `from` property. The `from`
property defines what e-mail address and name Anvil Connect will use in sending
e-mails. It must be in the form `Name <no-reply@example.com>`.

```json
{
  "mailer": {
    "from": "Hello World <test@gmail.com>",
    "view_engine": "hogan",
    "service": "Gmail",
    "auth": {
      "user": "test@gmail.com",
      "pass": "test"
    }
  },
}
```
