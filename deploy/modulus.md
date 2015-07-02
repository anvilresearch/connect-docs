> **WORK IN PROGRESS**. Bear with us while we develop Connect's docs to be as complete as possible. If you need immediate support, join our Gitter channel by clicking the link below, and we'll help as soon as we can. If you want to work with us to implement or develop Connect for you professionally, contact us on Gitter.
>
>[![Gitter](https://badges.gitter.im/anvilresearch/connect.svg)](https://gitter.im/anvilresearch/connect)
=======
<!-- WIP Guide to Installing on Modulus - Broken on Modulus's end right now. -->

**Notice**: Connect cannot run on Modulus's currently, as a dependency is not compatible with their system. We're working to get it up and running ASAP.

# Modulus

## Setup

To start the process of deploying Connect to a Modulus project, go to https://modulus.io.

#### Sign in or Create an account

Sign into your modulus account, or create a new one if you didn't previously have one.

## New project
Create a new Modulus project, with the following attributes:

* Input Project Name
* Node Project
* 193mb
<!-- ^^ This is the lowest memory available, which we used to test minimum requirements for Connect on Modulus. Update if minimum requirements are higher. -->
* Default server

## Create a new Anvil Connect project locally
Run  `sudo npm install -g anvil-connect` to install the Connect CLI. Once you've run this, you will have access to `nv`, Connect's CLI, globally on your computer or VM.

Then, to initialize an instance of Connect locally, run

```bash
mkdir anvil-connect
cd anvil-connect
nv init
```

## Setting up Modulus's CLI

Run `sudo npm install -g modulus` to install the Modulus's CLI. Once you've run this, you will have access to `modulus`, Modulus's CLI, globally on your computer or VM.

##### Login to Modulus via CLI
Run `modulus login`, and login with your Modulus account credentials.

// Other steps that are important
## Enabling SSL for your Connect instance on Modulus
SSL is a very important part of Connect - no Connect server should **ever** be run in production without SSL. To enable SSL for a connect instance on Modulus, follow the steps below.

#### Navigate to Administration tab

Go to the administration tab for the (account||server) Connect is running under.

##### Enable SSL Redirect
Enable the SSL redirect function of the (account||server) that Connect is running on. Once enabled, your Modulus instance will automatically use SSL to serve Connect.

## Spinning up a Redis Database for Connect

#### Sign up or log into Redis Labs

Go to https://redislabs.com/, and create a new account or log into an existing Redis Labs account.

## Create a New Redis Subscription

Create a new Redis Labs subscription:
* 30MB Free
* Input (Modulus||Connect) Information
* Create a password memorable password

# Add redis endpoint to production.json

In the production.json file of your Connect instance, add the correct Redis information that is provided on Redis Labs. Once the Redis Labs database is ready, you are ready to deploy.

## modulus deploy
Run `modulus deploy` and select the Connect instance you've set up from the CLI.
Run `NODE_ENV=production nv migrate` against local repo

Your Modulus instance running Connect is almost complete.

## Problems with Modulus.
Right now, Modulus doesn't work with one of the Connect dependencies - we're working on finding a solution to the problem, and will update this document as soon as it's ready.

If you would like to be notified of when it is working, [create an issue](https://github.com/anvilresearch/connect-docs/issues/new) in the anvilresearch/connect-docs repo and we will alert you when it is ready.

Additionally, if you would like professional support for your connect instance, contact us on our Gitter Channel:

[![Gitter](https://badges.gitter.im/anvilresearch/connect.svg)](https://gitter.im/anvilresearch/connect)
