<!-- TODO:
    [ ] - Rewrite "Learn More" text for links in Getting Started sections
-->

# Overview

## Why Anvil Connect?

The problem of auth is like an iceberg. On the surface users only see the button to "sign in with Facebook" or a simple password login form. This makes  it seem like auth should be easy and straightforward - just use some library and get on with building the app.

Auth quickly becomes more challenging when you have more than one app that you want to share user accounts and permissions across. It gets worse when you want to offer users a choice of several authentication methods. Now you have a many to many problem. These challenges get exponentially worse when you start publishing and consuming API data. Anvil Connect solves these problems by providing a single point of truth for identity and access management.



## How it works
<!-- Is this Connect or Connect's server that acts as a database? -->
Anvil Connect acts as a database of users, apps, services, permissions and connections to other authentication providers. When running the server, _you_ also become a provider of OAuth 2.0 and OpenID Connect (which is an authentication service that isn't a part of the OpenID service most people know). Connect's implementation of these allow for sharing user accounts between applications, and protects your APIs with with JSON Web Tokens. As such, third party developers can, if you choose to allow it, build apps that authenticate against your instance of Connect.

By default, Anvil Connect has the option to deligate user authentication to other identity providers. Out of the box, you can authenticate users with a growing list of third parties, including AngelList, Dropbox, Facebook, Foursquare, GitHub, Google, LinkedIn, Reddit, SoundCloud, Twitter, and WordPress. In addition, the local Connect server can authenticate users with standard email + password combinations within your instance of Connect. It never stores passwords, only a hash of the password.

<!--     Is the hash salted? ^^^^^^^^^^^^^^^^^^^^^^ -->

Even though Connect has many integrations out of the box, it is likely you'll need one that isn't one of the defaults. Luckily, you can easily extend Anvil Connect to support more providers using OAuth, OAuth 2.0, OpenID 2.0, or OpenID Connect. If that isn't enough, you can integrate virtually any existing Passport strategy or write your own custom auth code. The sky's the limit.



## Getting Started

### Server

Anvil Connect has a built-in server that runs independently of your apps and services. This allows for Connect to work in your app regardless of what stack it's running on. To start hacking on Connect, you'll first need to set up your own instance of the server in a development environment. Then you can configure, customize, and deploy.

[Learn more](/docs/connect-docs/server/)

### Clients

After setting up your server, the next step is to register apps and services to allow your users to sign in with. Once you’ve registered your Connect instance with the apps or services you want to give as an option for your users, you can add their respective libraries and configure them to use your new auth server.

[Learn more](/docs/connect-docs/clients/)

### Users

The Anvil Connect server is a database for users' identity. This identity can be unique to your app, serving as an authentication point for any other Connect app. Once initially identified by your Connect server, the user can attach other app or service authentication to their account.

An example of the flow:
```Markdown
| User arrives at site
|
|--- User registers account, with or without a third-party apps and services authenticating the user.
|  | --- User now has an account, and can add third-party app and service affiliations whenever they'd like to.
|  | --- User can sign into any Connect-enabled site with their account from your site.
```

<!-- Question: How are peope going to log into site 2 with site 1's connect? -->

[Learn more](/docs/connect-docs/users/)

### Permissions

With OAuth scopes and Role-based Access Control, you can define fine-grained permissions for users and clients.

[Learn more](/docs/connect-docs/permissions/)

### Tokens

Anvil Connect issues signed JSON Web Tokens. Implementing your own client libraries isn't complex. Understanding JSON Web Tokens exposes the internals of how Connect works.

[Learn more](/docs/connect-docs/tokens/)

### API

HTTP API reference.

### CLI

The CLI for Anvil Connect helps you manage users, clients, permissions, and more. This guide has all the details.

[Learn more](/docs/connect-docs/cli/)

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
