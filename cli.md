# CLI Reference

The `nvl` command aims to provide control over every aspect of your server. It should be run from the root of your project directory. You can get it by installing Anvil Connect globally via npm:

```bash
$ npm install -g anvil-connect
```



## Manage Resources

There are a set a CRUD commands for managing resources on the server including users, clients, roles, and scopes.

```bash
$ nvl ls <user|client|role|scope>
$ nvl get <user|client|role|scope> <_id|email>
$ nvl add <user|client|role|scope> <json>
$ nvl update <user|client|role|scope> <_id|email> <json>
$ nvl rm <user|client|role|scope> <_id|email>
```

## Manage Permissions

You can manage user and client RBAC permissions with `assign`, `revoke`, `permit`, and `forbid`.

```bash
$ nvl assign <email> <role>
$ nvl revoke <email> <role>
$ nvl permit <role> <scope>
$ nvl forbid <role> <scope>
```

## Convenience commands

The `uri` command is useful for quickly obtaining an authorization uri for experimentation and testing. This command logs a URI for the user and client to the console and also copies it to the clipboard.

```bash
$ nvl uri
```

You can quickly generate and decode JWT access tokens using the `token` and `decode` commands.

```bash
$ nvl token        # obtain an access token for a user
$ nvl token -c     # obtain an access token for a client
$ nvl decode JWT   # decode a JWT issued by your server
```

Register a user with a password based on your `gitconfig`. This can be useful for quickly creating an administrative user.

```bash
$ nvl signup

# OPTIONALLY ASSIGN ADMINISTRATIVE PRIVILEGES
$ nvl assign <email> authority
```

## Configuration

View Configured OpenID Provider Metadata.

```bash
$ nvl config
```

