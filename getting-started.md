## Getting Started

### Requirements

Anvil Connect works with the latest versions of [Node.js][nodejs] (0.12.x to 4.0.0 and higher) or
[io.js][iojs] (3.x.x) and [Redis][redis] (3.0.x) and is tested on
[Debian][debian], [Ubuntu][ubuntu], [Alpine][alpine] Linux distributions, and
Mac OS X.

The server and its dependencies can optionally run inside [Docker][docker]
containers. We recommend using Docker Compose, although it is not required.
[Installation instructions][docker-install] for Docker and related tools are
available on the Docker website. On Mac and Windows you can easily set up a
Docker environment using the [Docker Toolbox][docker-toolbox].

[nodejs]: https://nodejs.org
[iojs]: https://iojs.org
[redis]: http://redis.io
[debian]: https://www.debian.org
[ubuntu]: http://www.ubuntu.com
[alpine]: https://www.alpinelinux.org
[docker]: https://www.docker.com
[docker-install]: https://docs.docker.com/installation
[docker-toolbox]: https://www.docker.com/toolbox


### Initializing your project

To get started with Anvil Connect, install the `anvil-connect-cli` package
globally using npm.

```bash
$ npm install -g anvil-connect-cli
```

> **Note:** Some commands in the documentation will refer to the `nv` command.
As the `anvil-connect-cli` package is still in the process of becoming
feature-complete, you will also need to run
> ```bash
$ npm install -g anvil-connect
```
> to obtain the `nv` command. In the future, installing `anvil-connect` globally
will be deprecated.
>
> `nv` must be run inside the directory where Anvil Connect is configured, unlike
> `nvl`.

After this has completed successfully, you can generate all the files you'll
need to run your own custom auth server.

First, make a new directory and `cd` into it. Then, run `nvl init`.

```
$ mkdir myauthserver
$ cd myauthserver
$ nvl init
```

This command will prompt you for some essential information and choices about
your deployment.

With default options, `nv init` will generate all the files you
need to customize and run Anvil Connect, with or without Docker, Redis and
nginx, depending on your prompt selections.

If you choose to use nginx, you'll also be prompted to create a new self-signed
SSL certificate. You can opt out of any of the components other than Anvil
Connect.

```bash
# defaults to the name of the current directory
? What would you like to name your Connect instance?

# FQDN of the auth server
# This can be localhost, an IP address, or a (sub)domain
? What (sub)domain will you use? connect.example.io

# We recommend our default Docker setup, including
# support for running Anvil Connect, Redis, and nginx
# with Docker Compose. You can opt out of any or all
# of this if Docker isn't your cup of tea.
? Would you like to use Docker? Yes

# We can generate a Redis configuration and optional
# Dockerfile. You can also configure Anvil Connect to
# run against any Redis instance available over the
# network.
? Would you like to run Redis? Yes

# We recommend using nginx for SSL termination, load balancing,
# and caching of static assets. If you wish to employ other means
# answer "no".
? Would you like to run nginx? Yes

# With nginx or without it, be sure to use SSL in production
? Would you like to create a self-signed SSL cert? Yes

# If you opt "yes" to generating an SSL cert, you'll be
# prompted for the certificate subject information.
? Country Name (2 letter code) US
? State or Province Name (full name) South Dakota
? Locality Name (eg, city) Rapid City
? Organization Name (eg, company) Anvil Research, Inc
```

Once finished, you'll have a directory containing all the files needed to run
your auth server.

We've structured the package so that you can customize your server without
having to maintain a fork. You can add your own static assets, customize views
(HTML templates), integrate custom auth protocols, manage project-specific
dependencies, and keep all this, along with your configuration files, under
version control using git. This setup makes upgrading Anvil Connect as simple
as changing the version number in `connect/package.json` in most cases.


## Run

### Running locally

A Redis instance needs to running locally or accessible over the network. Before you
can run Anvil Connect, you'll also need to install npm and bower dependencies.

```bash
$ cd connect
$ npm install
$ bower install
```

You can run the server in `development` mode with:

```bash
$ node server.js
```

This will use the `config/development.json` settings. To run in production mode, set
the `NODE_ENV` environment variable to `production`.

```bash
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



### Running in Docker Containers

If you've opted to use Docker, Redis, and nginx, you should be ready to roll.
You can use Docker Compose to run a complete, production-ready set of
containers.

First, you'll want to ensure a few things are in order.

#### Windows and OS/X Users
On Windows and OSX, [Docker Toolbox](https://www.docker.com/toolbox),
[Docker Machine with VirtualBox](https://www.docker.com/docker-machine),
and [Boot2Docker](http://boot2docker.io/) all configure a VirtualBox VM to
run the Docker host.

If you're running one of these, then your project folder **must** exist within
the directory tree underneath ```/Users/``` (for OS/X) or ```c:\Users\```
(for Windows).  This is because boot2docker is used by all of these tools,
and by default it only maps the Users directory tree into the Docker VM.

If the project folder exists within your home directory or your ```Documents```
folder, then in most cases it will work fine.

If you must locate the project folder elsewhere, see this
[StackOverlow post](http://stackoverflow.com/questions/30586949/how-do-i-map-volume-outside-c-users-to-container-on-windows)
for a possible solution.

#### IP Address

If you're running on Linux, Anvil Connect will be bound to localhost and/or the
IP of your computer/server. Once the containers are running, you should be able
to reach the service at `https://0.0.0.0`, `https://127.0.0.1`, or 
`https://localhost`.


On Mac once you've installed the [Docker Toolbox][docker-toolbox] you can get
the IP address with the following command.

```bash
$ docker-machine ip default
```

#### /etc/hosts

To work with your Anvil Connect instance locally, it's a good idea to make an
entry in your `/etc/hosts` file. For example, on a Mac, you'll want to
associate the (sub)domain you provided the generator with the IP address of 
your Docker host.

```bash
192.168.99.100 connect.example.io
```

#### SSL Certificate

When you ran `nv init`, you may have opted to generate a self-signed SSL
certificate. The files were created in the `nginx/certs` directory of this
project. If you did not opt to generate these files, you'll need to provide
your own `nginx.key` and `nginx.crt` files in the `nginx/certs` directory.


#### Start Anvil Connect with Docker Compose

You can run these commands in the directory where you ran `nvl init`.

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

If you're having trouble reaching the running Anvil Connect instance, you can
view the logs for each type of container with the following command.

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

### [Configuring the server](server/configuration.md)
