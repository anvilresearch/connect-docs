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
nvl client:update [<id>] [--issuer | -i <issuer id>] [--trusted | -t] [--untrusted]
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

### client:token

```bash
nvl client:token [--issuer | -i <issuer id>]
```


## User Registration

### user:register

```bash
nvl user:register [--issuer | -i <issuer id>]
        [--name | -n <name>] [--given | -g <given name>]
        [--middle | -m <middle name>] [--family | -f <family name>]
        [--nickname | -k <nickname>] [--username | -u <preferred username>]
        [--profile | -p <profile url>] [--picture | -i <picture url>]
        [--website | -w <website url>] [--email | -e <email>]
```

### user:list

```bash
nvl user:list [--issuer | -i <issuer id>]
```

### user:info

```bash
nvl user:info [<id>] [--issuer | -i <issuer id>]
```

### user:update

```bash
nvl user:update [<id>] [--issuer | -i <issuer id>]
        [--name | -n <name>] [--given | -g <given name>]
        [--middle | -m <middle name>] [--family | -f <family name>]
        [--nickname | -k <nickname>] [--username | -u <preferred username>]
        [--profile | -p <profile url>] [--picture | -i <picture url>]
        [--website | -w <website url>] [--email | -e <email>]
```

### user:delete

```bash
nvl user:delete [<id>] [--issuer | -i <issuer id>]
```

### user:roles

```bash
nvl user:roles [<id>] [--issuer | -i <issuer id>]
```

### user:assign

```bash
nvl user:assign [<user id> <role name>] [--issuer | -i <issuer id>]
```

### user:revoke

```bash
nvl user:revoke [<user id> <role name>] [--issuer | -i <issuer id>]
```

### user:token

```bash
nvl user:token [--issuer | -i <issuer id>]
```

## Roles

### role:register

```bash
nvl role:register [<id>] [--issuer | -i <issuer id>] [--name | -n <name>]
```

### role:list

```bash
nvl role:list [--issuer | -i <issuer id>]
```

### role:info

```bash
nvl role:info [<id>] [--issuer | -i <issuer id>]
```

### role:update

```bash
nvl role:update [<id>] [--issuer | -i <issuer id>] [--name | -n <name>]
```

### role:delete

```bash
nvl role:delete [<id>] [--issuer | -i <issuer id>]
```

### role:scopes
### role:permit
### role:forbid


## Scopes

### scope:register

```bash
nvl scope:register [<id>] [--issuer | -i <issuer id>] [--name | -n <name>]
        [--description | -d <description>] [--restricted | -r]
```

### scope:list

```bash
nvl scope:list [--issuer | -i <issuer id>]
```

### scope:info

```bash
nvl scope:info [<id>] [--issuer | -i <issuer id>]
```

### scope:update

```bash
nvl scope:update [<id>] [--issuer | -i <issuer id>] [--name | -n <name>]
        [--description | -d <description>] [--restricted | -r]
```

### scope:delete

```bash
nvl scope:delete [<id>] [--issuer | -i <issuer id>]
```

## Version

```bash
nvl version
```
