## Get Started

### Requirements

Anvil Connect works with the latest versions of [Node.js](https://nodejs.org/)
(0.12.x) or [io.js](https://iojs.org/en/index.html) and
[Redis](http://redis.io/) (3.0.x). The server can optionally run inside
[Docker](https://www.docker.com/) containers. To run it with Docker or Docker
Compose you'll need to have them installed on your system.


### Initializing your project

To get started with Anvil Connect, install the `anvil-connect-cli` package
globally using npm.

```bash
$ npm install -g anvil-connect-cli
```

After this has completed successfully, you can generate all the files you'll
need to run your own custom auth server. First, make a new directory and `cd`
into it. Then, run `nvl init` and answer the prompts. With default options,
this command will generate all the files you need to customize and run Anvil
Connect, with or without Docker, Redis and nginx, depending on your prompt
selections. If you choose to use nginx, you'll also be prompted to create a new
self-signed SSL certificate. You can opt out of any of the components other
than Anvil Connect.

```
$ nvl init
? What would you like to name your Connect instance? myconnect
? What (sub)domain will you use? myconnect.example.com
? Would you like to use Docker? Yes
? Would you like to run Redis? Yes
? Would you like to run nginx? Yes
? Would you like to create a self-signed SSL cert? Yes
? Country Name (2 letter code) US
? State or Province Name (full name) South Dakota
? Locality Name (eg, city) Rapid City
? Organization Name (eg, company) Anvil Research, Inc.
```

Once finished, you'll have a directory containing all the files needed to run
your auth server. Anvil Connect itself is required as a dependency via npm.
We've structured the package so that you can customize your server without
having to maintain a fork. You can add your own static assets, customize views
(HTML templates), integrate custom auth protocols, manage project-specific
dependencies, and keep all this, along with your configuration files, under
version control. This setup makes upgrading Anvil Connect as simple as changing
the version number in `package.json` in most cases.


## Run

### Running in Docker Containers

If you've opted to use Docker, Redis, and nginx, you should be ready to roll.
To run a complete, production-ready set of containers you can use Docker
Compose. First, you'll want to ensure a few things.

#### IP Address

If you're running on Linux, the IP address Connect will be bound to is
localhost and/or the IP of your computer/server. Once the containers are
running, you should be able to reach the service at `https://0.0.0.0`.

On Mac and other systems you may need to install boot2docker in order to run
Docker containers locally. Once you've installed boot2docker, you can obtain
the IP address of your local Docker host by running:

```bash
boot2docker ip
```

#### /etc/hosts

To work with your Anvil Connect instance locally, it's a good idea to make an
entry in your `/etc/hosts` file. For example, on a Mac, you'll want to
associate the (sub)domain you provided the generator with `boot2docker` IP
address.

```bash
192.168.59.103 connect.example.io
```

#### RSA Key Pair


#### SSL Certificate

When you ran `nv init`, you may have opted to generate a self-signed SSL
certificate. The files were created in the `nginx/certs` directory of this
project. If you did not opt to generate these files, you'll need to provide
your own `nginx.key` and `nginx.crt` files.


#### Start Anvil Connect

```bash
$ docker-compose up -d
```

#### Stop

```
$ docker-compose stop [connect|nginx|redis]
```

#### Restart

```
$ docker-compose restart
```

#### View Logs

```
$ docker-compose logs <connect|nginx|redis>
```


### Building Custom Containers

By default, Docker Compose is configured to use images provided by Anvil
Research on Docker Hub. You can build images yourself by commenting out the
`image` property of a service in `docker-compose.yml` and uncommenting the
`build` property like so:

```yaml
connect:
  build: connect
  #image: anvilresearch/connect
  ...
```

While we highly recommend using the official images or provided Dockerfiles,
if necessary you can modify them to suit your requirements. You can also push
your own custom images to Docker Hub and use them to run Connect by referencing
them in the `image` property in `docker-compose.yml`.

```yaml
connect:
  #build: connect
  image: <dockerhubusername>/connect
```

### Running without Docker

#### Install Dependencies

Now you can install npm and bower dependencies, if you want to run Connect
locally.

```bash
$ npm install
$ bower install
```

#### Initializing the database

In current versions of Anvil Connect, database initialization is handled by the
server at run-time. See [Run](#Run) for details.

`nv migrate` has been deprecated.

#### Environments

#### Commands
There are two environments to run the Connect server in, `development`, and
`production`. The development server is for local testing, setup, and
development on Connect itself. The production environment should be used when
deployed to a live environment.

To run the authorization server in `development` mode, you can run the server
by simply starting it:

```bash
# The following are equivalent, any of them will start the development server
$ node server.js
$ npm start
```

To run the server in production, set `NODE_ENV` to `production`:

```bash
# The following are equivalent, any of them will start the production server
$ node server.js -e production
$ NODE_ENV=production node server.js
```

When Anvil Connect starts for the first time, it will check to see whether or
not the Redis server at the configured hostname and port has any data inside and
whether or not it contains data from a valid Anvil Connect instance.

If Anvil Connect detects any existing data in the database, and it is not from
an Anvil Connect instance, it will halt with the following error:

```
Redis already contains data, but it doesn't seem to be an Anvil Connect database.
If you are SURE it is, start the server with --no-db-check to skip this check.
```

If you are sure that there is no conflicting data in Redis, for example,
in the event that you may have edited Redis's data manually, start Anvil Connect
with the `--no-db-check` flag.

```bash
$ node server.js --no-db-check
```

