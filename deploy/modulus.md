> **WORK IN PROGRESS**. Bear with us while we develop Connect's docs to be as complete as possible. If you need immediate support, join our Gitter channel by clicking the link below, and we'll help as soon as we can. If you want to work with us to implement or develop Connect for you professionally, contact us on Gitter.
>
> Broken on Modulus's end right now. We're working to get it up and running ASAP.
>
>[![Gitter](https://badges.gitter.im/anvilresearch/connect.svg)](https://gitter.im/anvilresearch/connect)
=======
<!-- WIP Guide to Installing on Modulus - Broken on Modulus's end right now. -->

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
6. Navigate to Administration tab !important
6.1. Enable SSL Redirect
8 Go to redislabs.com
7.1. Create account or Login
7.2. New Redis Subscription
7.2.1. 30MB Free
7.2.2 Input Information
7.2.3 Create a password and REMEMBER IT: c0nn3ct
7.3 Add redis endpoint to production.json
8. modulus deploy
8.1. Select connect project
9. Run `NODE_ENV=production nv migrate` against local repo
