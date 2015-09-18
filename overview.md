<!-- TODO:
    [ ] - Rewrite "Learn More" text for links in Getting Started sections
-->

# Overview

## Why Anvil Connect?

The problem of auth is like an iceberg. On the surface users only see the button to "sign in with Facebook" or a simple password login form. From this perspective, auth should be easy and straightforward. Just grab a library and get on with building the app.
 
When you get beneath the surface, though, the problem is much bigger than it seems. Auth becomes challenging when you have more than one app and you want to share user accounts and permissions. It gets more complex when you want to offer users a choice of several authentication methods. These challenges multiply further when you start publishing and consuming API data. Anvil Connect solves these problems by providing a single point of truth for identity and access management.

#### Support us on [Bountysource](https://salt.bountysource.com/teams/anvilresearch)! 
We are 100% opensource, we appreciate the help!

## How it works
<!-- Is this Connect or Connect's server that acts as a database? -->
Anvil Connect acts as a database of users, apps, services, permissions and connections to other providers. When you run the server, you also become a provider of OAuth 2.0 and OpenID Connect. Connect allows you to share user accounts between applications and protect your APIs with JSON Web Tokens. It also means third party developers can, if you choose to allow it, build apps that authenticate against your instance of Connect.

Using Connect, you can delegate user authentication to other identity providers. Out of the box, you can connect users with a growing list of third parties, including AngelList, Dropbox, Facebook, Foursquare, GitHub, Google, LinkedIn, Reddit, SoundCloud, Twitter, and WordPress. The local Connect server can authenticate users with standard username + password combinations within your instance of Connect. It never stores passwords, only a hash of the password created with [bcrypt](https://www.npmjs.com/package/bcrypt).

Even though Connect has many integrations out of the box, it is likely you'll need one that isn't one of the defaults. Luckily, you can easily extend Anvil Connect to support more providers using OAuth, OAuth 2.0, OpenID 2.0, or OpenID Connect. If that isn't enough, you can integrate virtually any existing Passport strategy or write your own custom auth code. The sky's the limit.



## Getting Started

### Server

Anvil Connect runs independently of your apps and services. To start using it, you'll first need to set up your own instance of the server in a development environment. Then you can configure, customize, and deploy.

[Learn how to run your own Connect server](/docs/connect-docs/server/)

### Clients

After setting up your server, the next step is to register apps and services to allow your users to sign in with. Once you’ve registered your Connect instance with the apps or services you want to give as an option for your users, you can add their respective libraries and configure them to use your new auth server.

[Learn more](/docs/connect-docs/clients/)

### Users

The Anvil Connect server is a database for users' identity. This identity can be unique to your app, serving as an authentication point for any other Connect app. Once initially identified by your Connect server, the user can attach other app or service authentication to their account.

<!--
User notes:

* Shared across many apps.
* User data is based on OpenID Connect standard claims, which makes user data portable across identity providers.
* Standard claims enables federated identity.

--

Question: How are peope going to log into site 2 with site 1's connect?

-->

[Learn more](/docs/connect-docs/users/)

### Permissions

With OAuth scopes and Role-based Access Control, you can define fine-grained permissions for users and clients.

[Learn more](/docs/connect-docs/permissions/)

### Tokens

Anvil Connect issues signed JSON Web Tokens. Implementing your own client libraries isn't complex. Understanding JSON Web Tokens exposes the internals of how Connect works.

[Learn more](/docs/connect-docs/tokens/)

### API

Anvil Connect has a RESTful and HTTP API, which can utilized by any language, framework, app, or plugin. The API makes Connect completely platform agnostic, meaning you can use it with anything.

<!-- Note: Fill out with steps the user can take to use the API. -->

### CLI

The CLI for Anvil Connect helps you manage users, clients, permissions, and more. The command line tool is easy to use, with a basic set of commands that allow full control over your apps' integration with Connect.

[Learn more](/docs/connect-docs/cli/)

<!-- Note: Fill out -->

## Support

### Chat on Gitter

[![Gitter](https://badges.gitter.im/anvilresearch/connect.svg)](https://gitter.im/anvilresearch/connect)

Come say hello on [Gitter](https://gitter.im/anvilresearch/connect)! We love talking shop with Anvil Connect users :)

### Stack Overflow

Ask general usage questions on [Stack Overflow](http://stackoverflow.com/questions/tagged/anvil-connect) using the tag "anvil-connect".

### GitHub Issues

If you find errors or omissions in the docs, please [submit an issue](https://github.com/anvilresearch/connect-docs/issues) in the docs repository on GitHub. Report bugs directly in the repo for the code in question.

### Google Hangouts

Every Thursday at 9am Pacific time we get together to map out the future of the project, talk through specs, review code, and help each other ship. You're welcome to [join in](https://plus.google.com/hangouts/_/anvil.io/anvil-connect?authuser=0).
