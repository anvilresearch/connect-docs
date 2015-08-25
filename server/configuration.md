## Configuration

### Server Settings

Anvil Connect loads its configuration from a JSON file in the `config`
directory of the current working directory for the process. File names must
match the `NODE_ENV` value. If `NODE_ENV` is not set, `config/development.json`
will be loaded.

Setting | Type | Default | Description
------- | ---- | ------- | -----------
**issuer** | string | (none) | URI used to identify issuer of authentication
**port** | integer | 3000 | Port the Connect server is bound to
**cookie_secret** | string | (generated) | Secret string used to sign secure cookies
**session_secret** | string | (generated) | Secret string used to sign session ID cookies
**client_registration** | string | `scoped` | Type of client registration - `dynamic`, `token`, or `scoped` ([Explanation](../clients.md#registration))
**trusted_registration_scope** | string | `realm` | Scope used to identify trusted clients.

### [Configuring Redis](redis.md)

### [Configuring the mailer](mailer.md)

### [Configuring the logger](logger.md)

### Key pairs

If you generated a deployment repository with `nv init`, a new RSA key pair
will be generated for you in `config/keys`. This pair of files is required for
signing and verifying tokens. We recommend using the generated files. We have
set up key generation to lower the barrier to entry, as it is a tedious,
precise process to do by hand.

If you want or need to provide your own RSA key pair, you can obtain it using
OpenSSL and import them to the proper location, `config/keys/private.pem` for
the private key and `config/keys/public.pem` for the public key.

```
$ cd PROJECT_ROOT
$ mkdir -p config/keys
$ openssl genrsa -out config/keys/private.pem 2048
$ openssl rsa -pubout -in config/keys/private.pem -out config/keys/public.pem
```
