Node Usage
=====

Hardware requirements:

* 4 CPU cores,
* 8GB of RAM, and
* 256 GB of disk space.

For supporting higher API loads we recommend to use at least 8 CPU cores and
16GB of RAM.

Networking:

Chainweb node is configured to use port 1789 to communicate with other Chainweb
nodes.

*The P2P port must be reachable from the internet.*

This means that it must allow inbound requests from any IP address.

The HTTP REST API of Chainweb node is available on the host on port 1848.

Initialize Database
-------------------

```
docker compose up -d chainweb-initialize-db
docker logs chainweb-initialize-db --follow
```

The resulting database is *untrusted*. It is fine for use in testing and
non-critical applications.

The command can be skipped if the database has been initialized already.

Validate Database
-----------------

For production applications it is highly recommended to validate the database
after initialization.

```
docker compose up -d chainweb-validate-db
docker logs chainweb-validate-db --follow
```

The second command can take several hours depending on available hardware.
Currently, it takes about 6 hours on a cloud VM with eight CPU cores and eight
GB of RAM. Adding more CPU cores will speed up the process.

NOTE: The chainweb database validation step does not work with testnet.

Run Chainweb Node
-----------------

Prerequisite: an initialized and possibly validated database.

```
docker compose up -d
```

The service API of the node is available on the docker host at port 1848.

Options
-------

By default the node runs in the Kadena mainnet. To run a node in the Kadena
testnet define the `KADENA_NETWORK` variable in an `.env` file:

```
cat >> .env <<EOF
KADENA_NETWORK=testnet04
```

Database synchronization takes less time when the database is synchronized
from a geographically close location, ideally, in the same data center:

```
cat >> .env <<EOF
DB_SYNC_SERVER=IP_ADDRESS_OR_DOMAIN_NAME_OF_OTHER_NODE
```

If you already have a node running to you can make its database available for
remote synchronization as follows:

```
docker compose up -d chainweb-db-rsync
```

By default the P2P port is set to `1789` and the service port is set to `1848`.
To change these ports, define the environment variables `P2P_PORT` and
`SERVICE_PORT` in an `.env` file. The ports must be different values.

```
cat >> .env <<EOF
P2P_PORT=MY_P2P_PORT
SERVICE_PORT=MY_SERVICE_PORT
```
