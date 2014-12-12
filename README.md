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

Bind the relevant redis service and at the new redis environment variables. We need the redis host, port, and password. In this case, they're `10.1.0.123`, `52174`, `abc`, respectiviely:

```bash
$ cf bind-service redis-commander config-redis-datasync-datastore
$ cf env redis-commander
Getting env variables for app redis-commander in org Pivotal / space datasync as admin...
OK

System-Provided:
{
  "VCAP_SERVICES": {
    "p-redis": [
      {
        "credentials": {
          "host": "10.1.0.123",
          "password": "abc",
          "port": 52174
        },
        "label": "p-redis",
        "name": "config-redis-datasync-datastore",
        "plan": "development",
        "tags": [
          "pivotal",
          "redis"
        ]
      }
    ]
  }
}
No user-defined env variables have been set
```

Push the app again, but in this case pass in the necessary redis information in a custom start command. Look for a url logged at the end of the push process:

```bash
$ cf push redis-commander -c "node bin/redis-commander.js --redis-port 52174 --redis-host 10.1.0.123 --redis-password abc"
...
urls: redis-commander.my-cf-host.com
...
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
