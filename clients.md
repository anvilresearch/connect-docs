# Clients


After setting up your Anvil Connect server, the next step is to register apps and services, called clients, to obtain credentials and configure relevant [properties](#client-properties), depending on the type of client. Once you've done this, you can add a library dependency to your client code and configure it to use your auth server and credentials.


## Client Types

### Trusted Clients

As an identity provider, Anvil Connect can optionally authenticate users of third party apps against your auth server. When users come from another developer's app, the server prompts users to authorize access to their account. This is important when sharing data outside of your own realm, but unnecessary for your own apps. To streamline the user experience, you can register "trusted" apps.


### Application Type

Anvil Connect supports three kinds of clients: `web`, `native`, and `service`. The type of client is determined by the [application_type](#application-type) property, which defaults to `web`. `web` clients can be server side apps like you might build with Rails or HTML5 apps that run entirely in the browser. `native` clients include iOS, Android, desktop, and CLI apps. `service` type clients include RESTful and realtime APIs. Users typically don't interact directly with services. Use this type if your client will acquire client-only credentials without user intervention.


## Registration

Registering a client creates a set of credentials that are required for interacting with Anvil Connect. You can set [properties](#client-properties) on a client that contain helpful information and also determine the way that client works depending on the context.


Although not strictly required, we recommend setting (at a minimum) `client_name` and `client_uri` for all clients. Web and native applications require one or more redirect_uris to be configured and should also define at least one `post_logout_redirect_uri` as well. When registering your own apps as clients, we recommended setting `trusted` to "true".

There are three ways to register clients with Anvil Connect. You can use the Anvil Connect CLI, the Dynamic Registration endpoint, or the REST API.

### CLI command

The quickest way to register a client is with the `nvl` CLI tool. Run it from the root of your project.


```bash
$ nvl add client '{
  "client_name": "Example App",
  "default_max_age": 36000,
  "redirect_uris": ["http://localhost:9000/callback.html"],
  "post_logout_redirect_uris": ["http://localhost:9000"],
  "trusted": "true"
}'
```

### Dynamic Registration

The `/register` endpoint is configured by default to require an access token issued with `realm` scope. To use it you can either obtain a token with that scope or configure the server's [client_registration](http://localhost:1313/docs/connect-docs/server/#client-registration) property to be less restrictive.

The body of a client registration request may contain any of the metadata values specified in [OpenID Connect Dynamic Client Registration 1.0](http://openid.net/specs/openid-connect-registration-1_0.html), [Section 2. Client Metadata](http://openid.net/specs/openid-connect-registration-1_0.html#ClientMetadata). Values not understood by the server are ignored. Only a valid `redirect_uris` value is required.

#### Anonymous Request

```bash
$ curl -X POST https://connect.example.com/register
       -H 'Content-Type: application/json'
       -d '{
            "client_name": "Triangular Pretzel",
            "redirect_uris": ["https://app.example.com/callback"]
          }'
```



#### Authorized Request

```bash
$ curl -X POST https://connect.example.com/register
       -H 'Content-Type: application/json'
       -H 'Authorization: Bearer <TOKEN>'
       -d '{
            "client_name": "Triangular Pretzel",
            "redirect_uris": ["https://app.example.com/callback"]
          }'
```

#### Response

In addition to client metadata, the registration response contains a `registration_client_uri` specifying the configuration endpoint for this client, and a `registration_access_token` which must be used as a bearer token to read and update the client configuration at that endpoint.

```bash
HTTP/1.1 201 Created
cache-control: no-store
pragma: no-cache
content-type: application/json

{
  "client_id":"e090a523-2ea8-43c8-b012b58ae60ce25d",
  "client_name":"Example",
  "application_type":"web",
  "redirect_uris":["https://app.example.com/callback"],
  "token_endpoint_auth_method": "client_secret_basic",
  "client_secret": "3ba1dbffa4402b7a6a76"
  "client_id_issued_at":1433960696883,
  "registration_client_uri":"https://connect.example.com/register/e090a523-2ea8-43c8-b012-b58ae60ce25d",
  "registration_access_token":"eyJhbGciOiJSUzI1NiJ9.eyJ***2fQ.UmN***kE0"
}
```

### REST API

### Registration permissions

Anvil Connect can be configured for three types of client registration: `dynamic`, `token`, or `scoped`, each being more restrictive than the previous option. The default `client_registration` type is `scoped`. Trusted clients require additional scope to register. This can be configured with the `trusted_registration_scope` setting, which defaults to `realm`.

#### Dynamic Client Registration

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


#### Token-restricted Registration

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

#### Scoped Registration

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

## Client Properties

#### redirect_uris

<span class="label label-danger">Required</span>

Array of URIs that can be used by the server for redirecting users back to the client app after authentication. The `redirect_uri` parameter in an auth request must match one of these values exactly.

```
"redirect_uris": [
  "https://app.example.com/callback",
  "https://app.example.com/callback.html"
]
```

#### application_type

Three kinds of clients can be registered with Anvil Connect. The client type is determined by setting the `application_type` property of a client. Values can include `web`, `native`, and `service`.

```
"application_type": "web"
```

#### client_name

Name of the client that may be presented to the user during authentication.

```
"client_name": "Shiny App"
```

#### logo_uri

URI of an image that may be presented to the user during authentication.

```
"logo_uri": "http://example.com/image.jpg"
```

#### client_uri

URI of the application or information about the application.

```
"client_uri": "http://app.example.com"
```

#### default_max_age

Integer representing time in seconds before tokens will expire by default. This can be overridden by the `max_age` authorization parameter.

```
"default_max_age": 36000
```

#### post_logout_redirect_uris

Array of URIs that can be used by the server for redirecting users back to the client app after logout. The `post_logout_redirect_uri` parameter in an logout request must match one of these values exactly.

```
"post_logout_redirect_uris": [
  "https://app.example.com"
]
```

#### trusted

The string `true` indicates a client is part of your security realm. Any other value (including no value) indicates the client should be treated as a third party.

```
"trusted": "true"
```

#### default_client_scope

Array of strings used to specify scope requested for client access tokens issued by a `client_credentials` grant.

```
"default_client_scope": [
  "profile",
  "realm"
]
```

#### scopes

Array of strings. The `scopes` setting determines which scopes are required to access an app.

```
"scopes": [
  "photos",
  "activity"
]
```



## Permissions

### Scopes

When you want to require a user to have permission to access an app, you can define a `scopes` setting for the client. If this property contains any scopes, the user must be authorized to access those scopes via roles.

### RBAC

Clients can be assigned roles which grant the client permission to access scopes associated with the role. This is useful for API keys (two-legged OAuth).

You can assign a role to a client using the CLI.

```bash
# -c indicates the assignment should be made to a client
$ nvl assign -c <CLIENT_ID> <ROLE>
```

## Client Libraries

#### JavaScript

* [JavaScript](https://github.com/anvilresearch/connect-js)
* [AngularJS](https://github.com/anvilresearch/connect-js)

#### Node.js

* [Node.js](https://github.com/anvilresearch/connect-nodejs)
* [Passport OpenID Connect Strategy](https://github.com/jaredhanson/passport-openidconnect)

#### PHP

* [OpenID Connect PHP](https://github.com/jumbojett/OpenID-Connect-PHP)

<!--
#### Java

#### Ruby

#### Python

#### ...
-->

## Supported Software

* [Drupal](https://www.drupal.org/project/openid_connect)
* [Wordpress](https://github.com/jumbojett/Wordpress-OpenID-Connect-Login)

<!--
#### Ghost
#### Nodebb
-->
