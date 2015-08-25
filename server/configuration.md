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
will be generated for you in `connect/config/keys`. This pair of files is 
required for signing and verifying tokens. If the server does not find key 
pairs when starting, it will attempt to generate them for you using the OpenSSL 
package installed on your system. Mac and most Unix and Linux based systems 
include OpenSSL by default. You can also [install it on Windows][ssl-windows].

[ssl-windows]: https://slproweb.com/products/Win32OpenSSL.html

If you want or need to provide your own RSA key pair, you can obtain it using
OpenSSL and import them to the proper location, `connect/config/keys/private.pem` 
for the private key and `connect/config/keys/public.pem` for the public key.

```
$ cd PROJECT_ROOT
$ mkdir -p connect/config/keys
$ openssl genrsa -out connect/config/keys/private.pem 4096
$ openssl rsa -pubout -in connect/config/keys/private.pem -out connect/config/keys/public.pem
```
