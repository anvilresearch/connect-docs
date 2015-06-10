# Overview

## Why Anvil Connect?

The problem of auth is like an iceberg. On the surface users only see the button to "sign in with Facebook" or a simple password login form. It seems like it should be easy and straightforward. Just use some library and get on with building a great app.

Auth quickly becomes more challenging when you have more than one app and you want to share user accounts and permissions. It gets worse when you want to offer users a choice of several authentication methods. Now you have a many to many problem. These challenges multiply further when you start publishing and consuming API data. Anvil Connect solves these problems by providing a single point of truth for identity and access management.



## How it works

Anvil Connect acts as a database of users, apps, services, permissions and connections to other providers. When you run the server, you also become an OAuth 2.0 and OpenID Connect provider. That means you can share user accounts between applications and protect your APIs with JSON Web Tokens. It also means third party developers can (if you choose to allow it) build apps that authenticate using your domain.

Anvil Connect can delegate user authentication to other identity providers. Out of the box, you can authenticate users with a growing list of third parties, including AngelList, Dropbox, Facebook, Foursquare, GitHub, Google, LinkedIn, Reddit, SoundCloud, Twitter, and WordPress. In addition, the server can authenticate users locally with passwords. It never stores passwords, only a hash of the password.

You can easily extend Anvil Connect to support more providers using OAuth, OAuth 2.0, OpenID 2.0, or OpenID Connect. If that isn't enough, you can integrate virtually any existing Passport strategy or write your own custom auth code. The sky's the limit.



## Getting Started

### Server

Anvil Connect is a server that runs independently of your apps and services. To start using it, you'll first need to set up your own instance. Then you can configure, customize, and deploy.

[Learn more](/docs/connect-docs/server/)

### Clients

After setting up your server, the next step is to register apps and services. Once you’ve done this, you can add a library code and configure it to use your new auth server.

[Learn more](/docs/connect-docs/clients/)

### Users

Your Anvil Connect server is a database for user identities. Learn how to work with users in Anvil Connect.

[Learn more](/docs/connect-docs/users/)

### Permissions

With OAuth scopes and Role-based Access Control, you can define fine grained permissions for users and clients.

[Learn more](/docs/connect-docs/permissions/)

### Tokens

Anvil Connect issues signed JSON Web Tokens. Here's what you need to know to implement a client library or just learn how it works under the hood.

[Learn more](/docs/connect-docs/tokens/)

### API

HTTP API reference.

### CLI

The CLI for Anvil Connect helps you manage users, clients, permissions, and more. This guide has all the details.

[Learn more](/docs/connect-docs/cli/)

## Support

### Chat on Gitter

Come say hello on [Gitter](https://gitter.im/anvilresearch/connect)! We love talking shop with Anvil Connect users :)

### Stack Overflow

Ask general usage questions on [Stack Overflow](http://stackoverflow.com/questions/tagged/anvil-connect) using the tag "anvil-connect".

### GitHub Issues

If you find errors or omissions in the docs, please [submit an issue](https://github.com/anvilresearch/connect-docs/issues) in the docs repository on GitHub. Report bugs directly in the repo for the code in question.

### Google Hangouts

Every Thursday at 9am Pacific time we get together to map out the future of the project, talk through specs, review code, and help each other ship. You're welcome to [join in](https://plus.google.com/hangouts/_/anvil.io/anvil-connect?authuser=0).

