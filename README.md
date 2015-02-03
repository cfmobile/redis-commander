This is a version of Redis Commander that runs on Cloud Foundry.

Clone and push the app without starting it:

```bash
$ git clone https://github.com/cfmobile/redis-commander.git
$ cd redis-commander
$ cf push redis-commander --no-start
```

Using `cf-cli`, list the available services and look for redis instance you are interested in. In this case we will use `config-redis-datasync-datastore`. Our example assumes your `cf-cli` session is already logged in as admin:

```bash
$ cf services
Getting services in org Pivotal / space datasync as admin...
OK

name                                 service   plan          bound apps
mysql-datasync-authentication        p-mysql   100mb-dev     datasync-authentication
mysql-datasync-dashboard             p-mysql   100mb-dev     datasync-dashboard
config-redis-datasync-datastore      p-redis   development   datasync-datastore
datastore-redis-datasync-datastore   p-redis   development   datasync-datastore
```

Bind the relevant redis service to the app:

```bash
$ cf bind-service redis-commander config-redis-datasync-datastore
```

Push the app again, this time allowing it to start:

```bash
$ cf push redis-commander -c "node bin/redis-commander.js"
…
urls: redis-commander.my-cf-host.com
…
```

Now you should be able to access `redis-commander.my-cf-host.com`.

# Redis Commander

Redis management tool written in node.js

# Install and Run

```bash
$ npm install -g redis-commander
$ redis-commander
```

# Usage

```bash
$ redis-commander --help
Options:
  --redis-port                    The port to find redis on.         [string]
  --redis-host                    The host to find redis on.         [string]
  --redis-socket                  The unix-socket to find redis on.  [string]
  --redis-password                The redis password.                [string]
  --redis-db                      The redis database.                [string]
  --http-auth-username, --http-u  The http authorisation username.   [string]
  --http-auth-password, --http-p  The http authorisation password.   [string]
  --port, -p                      The port to run the server on.     [string]  [default: 8081]
  --address, -a                   The address to run the server on   [string]  [default: 0.0.0.0]
```
