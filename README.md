_This is a version of Redis Commander that runs on Cloud Foundry._

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

# Deploy to Cloud Foundry

Clone and push the app without starting it:

```bash
$ git clone https://github.com/cfmobile/redis-commander.git
$ cd redis-commander
$ cf push redis-commander --no-start -m 256M -c "node bin/redis-commander.js --http-u admin --http-p admin"
```

Using `cf-cli`, list the available services and look for redis instance you are interested in. In this case we will use `datasync-datastore-redis-config`. Our example assumes your `cf-cli` session is already logged in as a space developer:

```bash
$ cf services
Getting services in org system / space datasync as admin...
OK

name                              service   plan           bound apps   
datasync-authentication-mysql     p-mysql   100mb-dev      datasync-authentication   
datasync-datastore-redis-config   p-redis   dedicated-vm   datasync-datastore   
datasync-dashboard-mysql          p-mysql   100mb-dev      datasync-dashboard   
datasync-datastore-redis-data     p-redis   dedicated-vm   datasync-datastore
```

Bind the relevant redis service to the app:

```bash
$ cf bind-service redis-commander datasync-datastore-redis-config
```

Start the application:

```bash
$ cf restart redis-commander
…
urls: redis-commander.<my-cf-host-prefix>.<com>
…
```

Now you should be able to access `https://redis-commander.<my-cf-host-prefix>.<com>`. Login with 'admin':'admin' username and password.

