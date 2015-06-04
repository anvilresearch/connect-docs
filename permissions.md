# Permissions

## Roles

## Scope

## Users

## Clients

## Dynamic Registration

Anvil Connect can be configured for three types of client registration: `dynamic`, `token`, or `scoped`, each being more restrictive than the previous option. The default `client_registration` type is `scoped`. Trusted clients require additional scope to register. This can be configured with the `trusted_registration_scope` setting, which defaults to `realm`.

### Dynamic Client Registration

With `client_registration` set to `dynamic`, any party can register a client with the authorization server. Optionally, a bearer token may be provided in the authorization header per RFC6750. If a valid access token is presented with a registration request, the client will be associated with the user represented by that token.

```json
{
  // ...
  "client_registration": "dynamic",
  // ...
}
```

The following table indicates expected responses to *Dynamic Client Registration* requests.

| trusted | w/token | w/scope | response  |
|:-------:|:-------:|:-------:|----------:|
|         |         |         | 201       |
| x       |         |         | 403       |
|         | x       |         | 201       |
| x       | x       |         | 403       |
| x       | x       | x       | 201       |
|         | x       | x       | 201       |


### Token-restricted Registration

Client registration can be restricted so that a valid user access token is required by setting `client_registration` to `token`. In this case, any request without a token will fail. As with *Dynamic Client Registration*, in order to register a trusted client, the access token must have sufficient scope.

```json
{
  // ...
  "client_registration": "token",
  // ...
}
```

| trusted | w/token | w/scope | response  |
|:-------:|:-------:|:-------:|----------:|
|         |         |         | 403       |
| x       |         |         | 403       |
|         | x       |         | 201       |
| x       | x       |         | 403       |
| x       | x       | x       | 201       |
|         | x       | x       | 201       |

### Scoped Registration

Third party registration can be restricted altogether with the `scoped` `client_registration` setting. In this case, all registration requires a prescribed `registration_scope`.

```json
{
  // ...
  "client_registration": "scoped",
  // ...
}
```

| trusted | w/token | w/scope | response  |
|:-------:|:-------:|:-------:|----------:|
|         |         |         | 403       |
| x       |         |         | 403       |
|         | x       |         | 403       |
| x       | x       |         | 403       |
| x       | x       | x       | 201       |
|         | x       | x       | 201       |


