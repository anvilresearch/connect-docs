# Clients


After setting up your Anvil Connect server, the next step is to register your apps and services (clients) to obtain credentials and configure relevant [properties](#client-properties), depending on the type of client. Once you've done this, you can add a library dependency to your client code and configure it to use your auth server and credentials.


## Client types

Anvil Connect supports three kinds of clients: `web`, `native`, and `service`. The type of client is determined by the [application_type](#application-type) property, which defaults to `web`.  Although not strictly required, we recommend setting (at a minimum) `client_name` and `client_uri` for _all_ clients. Web and native applications require one or more `redirect_uris` to be configured and should also define at least one `post_logout_redirect_uri` as well.

### application_type

### trusted


## Registration

There are three ways to register clients with Anvil Connect. You can use the Anvil Connect CLI, the Dynamic Registration endpoint, or the REST API.

### CLI commands

```bash
$ nv add client '{"key":"value", ...}'
```

### Register endpoint

The `/register` endpoint is configured by default to require an access token issued with `realm` scope. To use it you can either obtain a token with

The body of a client registration request may contain any of the metadata values specified in [OpenID Connect Dynamic Client Registration 1.0](http://openid.net/specs/openid-connect-registration-1_0.html), [Section 2. Client Metadata](http://openid.net/specs/openid-connect-registration-1_0.html#ClientMetadata). Values not understood by the server are ignored. Only a valid `redirect_uris` value is required.

#### Anonymous Request

```bash
POST /register HTTP/1.1
Content-Type: application/json
Host: your.authorization.server

{
  "client_name":"Triangular Pretzel",
  "redirect_uris":["https://example.com/callback"]
}
```



#### Authorized Request

```bash
POST /register HTTP/1.1
Content-Type: application/json
Host: your.authorization.server

{
  "client_name":"Triangular Pretzel",
  "redirect_uris":["https://example.com/callback"]
}
```

#### Response

In addition to client metadata, the registration response contains a `registration_client_uri` specifying the configuration endpoint for this client, and a `registration_access_token` which must be used as a bearer token per [RFC6750](http://tools.ietf.org/html/rfc6750) to access that resource.

```bash
HTTP/1.1 201 Created
cache-control: no-store
pragma: no-cache
content-type: application/json

{
  "client_id":"e090a523-2ea8-43c8-b012b58ae60ce25d",
  "client_name":"Example",
  "application_type":"web",
  "redirect_uris":["https://example.com/callback"],
  "client_id_issued_at":1398534329550,
  "registration_client_uri":"https://accounts.anvil.io/register/e090a523-2ea8-43c8-b012-b58ae60ce25d",
  "registration_access_token":"eyJhbGciOiJSUzI1NiJ9.eyJ***2fQ.UmN***kE0"
}
```

### REST API

## Client Properties

#### redirect_uris
#### response_types
#### grant_types
#### application_type


Three kinds of clients can be registered with Anvil Connect. The client type is determined by setting the `application_type` property of a client. Values can include `web`, `native`, and `service`.

##### web

Web clients can be server side apps like you might build with Rails or HTML5 apps that run entirely in the browser.

##### native

Native clients include iOS, Android, desktop, and CLI apps.

##### service

Service type clients include RESTful and realtime APIs. Users typically don't interact directly with services. Use this type if your client will acquire client-only credentials without user intervention.


#### contacts
#### client_name
#### logo_uri
#### client_uri
#### token_endpoint_auth_method
#### default_max_age
#### post_logout_redirect_uris
#### client_secret
#### trusted
#### default_client_scope
#### scopes




## Permissions

### Scopes
### RBAC

## Libraries

#### JavaScript

#### Node.js

#### Ruby

#### Python

#### PHP

#### Java

#### ...

## Software

#### Drupal
#### Ghost
#### Nodebb
#### Wordpress

