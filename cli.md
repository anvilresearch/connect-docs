# nvl

The `nvl` command aims to help with deploying, managing, and maintaining Anvil Connect servers. You can get it by installing Anvil Connect globally via npm:

```bash
$ npm install -g anvil-connect-cli
```

## Terminology

- **Issuer** - an Anvil Connect server

## Initializing a project

_[See the getting started guide](getting-started.md#initializing-your-project)_

## Setting up a server for administration

In order to administer an Anvil Connect server, you need

- A registered, administrator ("authority") user
- A client registration entry for the CLI tool

### Fresh install

The `nvl setup` command takes care of registering an administrator account and registering the CLI client with an Anvil Connect that has no administrator user set up yet.

```
$ nvl setup https://connect.example.com --token-file path/to/connect/keys/setup.token
? Choose an email admin@example.com
? Choose a password ********
? Choose a name for this configuration connect.example.com
? Choose an ID for this configuration connect-example-com
Your setup is complete. You may now log in with `$ nvl login`.
```

### Existing install

If you already have a CLI registered with an Anvil Connect server, you can add the existing registration using `nvl issuer:add`.

```
$ nvl issuer:add
? Enter the issuer URI https://connect.example.com
? Enter the client ID 0a1b2c3d4e5f6-7a0b-1c2d-3e4f5a6b7c8d
? Enter the client secret 0a1b2c3d4e5f6a0b1c2d
? Enter the redirect URI https://connect.example.com
? Choose a name for this configuration My OIDC Provider
? Choose an ID for this configuration example-oidc-provider
Added issuer. You may now log in with `$ nvl login`.
```

You can replace any of the prompts with their command-line argument equivalents:

```
 nvl issuer:add [<issuer uri>] [--client-id | -c <id>]
        [--client-secret | -s <secret>] [--redirect-uri | -r <uri>]
        [--name | -n <config name>] [--id | -i <config id>]
```


## CLI User Authentication

### login

Running the login command will first prompt you to select an issuer.

```bash
nvl login
? Select an Anvil Connect instance (Use arrow keys)
❯ connect.anvil.io (connect-anvil-io)
  laptop-connect.anvil.io (laptop-connect-anvil-io) 
```

After you select an issuer, you'll be prompted for your email and password to login.

```bash
? Select an Anvil Connect instance connect.anvil.io (connect-anvil-io)
Selected issuer connect.anvil.io (https://connect.anvil.io)
? Enter your email smith@anvil.io
? Enter your password **********
You have been successfully logged in to connect.anvil.io  
```

Once you've logged into an issuer, you can run other commands that require authentication.



## Client Registration

### client:register

```bash
nvl client:register [--issuer | -i <issuer id>] [--trusted | -t]
         [--name | -n <name>] [--uri | -u <uri>]
         [--logo-uri | -l <logo uri>] [--application-type | -a <app type>]
         [--response-type | -r <response type>] [--grant-type | -g <grant type>]
         [--default-max-age | -d <seconds>] [--redirect-uri | -s <redirect uri>]
         [--post-logout-redirect-uri | -p <post logout redirect uri>]
```

### client:list

```bash
nvl client:list [--issuer | -i <issuer id>]
```

### client:info

```bash
nvl client:info [<id>] [--issuer | -i <issuer id>]
```

### client:update

```bash
nvl client:update [<id>] [--issuer | -i <issuer id>] [--trusted | -t]
         [--name | -n <name>] [--uri | -u <uri>]
         [--logo-uri | -l <logo uri>] [--application-type | -a <app type>]
         [--response-type | -r <response type>] [--grant-type | -g <grant type>]
         [--default-max-age | -d <seconds>] [--redirect-uri | -s <redirect uri>]
         [--post-logout-redirect-uri | -p <post logout redirect uri>]
```

### client:delete

```bash
nvl client:delete [<id>] [--issuer | -i <issuer id>]
```



# Old CLI reference

**NOTE:** The `nv` command is being phased out in favour of `nvl`. This reference will remain during the transitional phase for functionality not yet supported by `nvl`.

----

The `nv` command aims to provide control over every aspect of your server. It should be run from the root of your project directory. You can get it by installing Anvil Connect globally via npm:

```bash
$ npm install -g anvil-connect
```



## Manage Resources

There are a set a CRUD commands for managing resources on the server including users, clients, roles, and scopes.

```bash
$ nv ls <user|client|role|scope>
$ nv get <user|client|role|scope> <_id|email>
$ nv add <user|client|role|scope> <json>
$ nv update <user|client|role|scope> <_id|email> <json>
$ nv rm <user|client|role|scope> <_id|email>
```

## Manage Permissions

You can manage user and client RBAC permissions with `assign`, `revoke`, `permit`, and `forbid`.

```bash
$ nv assign <email> <role>
$ nv revoke <email> <role>
$ nv permit <role> <scope>
$ nv forbid <role> <scope>
```

## Convenience commands

The `uri` command is useful for quickly obtaining an authorization uri for experimentation and testing. This command logs a URI for the user and client to the console and also copies it to the clipboard.

```bash
$ nv uri
```

You can quickly generate and decode JWT access tokens using the `token` and `decode` commands.

```bash
$ nv token        # obtain an access token for a user
$ nv token -c     # obtain an access token for a client
$ nv decode JWT   # decode a JWT issued by your server
```

Register a user with a password based on your `gitconfig`. This can be useful for quickly creating an administrative user.

```bash
$ nv signup

# OPTIONALLY ASSIGN ADMINISTRATIVE PRIVILEGES
$ nv assign <email> authority
```

## Configuration

View Configured OpenID Provider Metadata.

```bash
$ nv config
```

