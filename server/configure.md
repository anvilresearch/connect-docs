## Configure

### JSON files

Anvil Connect loads its configuration from a JSON file in the `config`
directory of the current working directory for the process. File names must
match the `NODE_ENV` value. If `NODE_ENV` is not set, `config/development.json`
will be loaded.

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


### Server Settings

##### issuer

**Type:** string

**Use:** URI used to identify issuer of authentication

**Description:** Fully qualified base URI of the authorization server; e.g.,
<code>https://accounts.anvil.io</code>


##### port

**Type:** integer

**Use:** port # the Connect server is run under

**Description:** An integer value representing the port the server will be bound
to, unless a <code>PORT</code> environment variable is provided. Defaults to
<code>3000</code>.

##### cookie_secret

**Type:** string

**Use:** signing cookies  <!-- under what scope? -->

**Description:** A string used for signing cookies. When you initialize a
project, this value is generated for each of your environments. Treat it as
confidential and always use separate values for each project and environment.

##### session_secret

**Type:** string

**Use:** signing session cookies

**Description:** A string used for signing session ID cookies. When you
initialize a project, this value is generated for each of your environments.
Treat it as confidential and always use separate values for each project and
environment.

##### client_registration

**Type:** string - options: `dynamic`, `token`, or `scoped`

**Use:** Assigning the type of client registration Connect uses

**Description:** Anvil Connect can be configured for three types of client
registration: `dynamic`, `token`, or `scoped`, each being more restrictive than
the previous option. The default `client_registration` type is `scoped`. For
more details, see the section titled
[Client Registration](#client-registration-1).

##### trusted_registration_scope

**Type:** string - options: `realm`, Connect operator's own registered scope(s)

**Use:** signing session cookies

**Description:** `trusted_registration_scope` signals if a client is trusted or
not - trusted clients require additional scope to register. It defaults to
`realm`.

