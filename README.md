Supported tags and respective `Dockerfile` links
================================================

-	`1.6.1`, `1.6`: [Dockerfile](https://github.com/ofkindness/docker-pgbouncer/blob/master/1.6.1/Dockerfile)
-	`1.7.2`, `1.7`, `latest`: [Dockerfile](https://github.com/ofkindness/docker-pgbouncer/tree/master/1.7.2/Dockerfile)

For more information about this image and its history, please see [the relevant manifest `pgbouncer/manifest` file](https://github.com/ofkindness/docker-pgbouncer/tree/master/manifest).

What is PgBouncer?
==================

PgBouncer is an open source lightweight connection pooler for PostgreSQL. Provides a low memory requirements (2K per connection by default) and several levels of brutality when rotating connections:

-	session pooling
-	statement pooling
-	transaction pooling

Supports:

-	online reconfiguration for most of the settings
-	restart/upgrade without dropping client connections
-	multi server backend, the destination databases can reside on different hosts

Caveats:

-	Since PgBouncer implements V3 protocol only, backend version must be >= 7.4.
-	Supported [SQL features](https://pgbouncer.github.io/features.html) are limited within pooling modes

You can find more information about pgbouncer configuration and usage [here](https://pgbouncer.github.io/faq.html)

How to use this image
=====================

You can start a pgbouncer instance directly with [official postgres image container](https://hub.docker.com/_/postgres/) link:

```console
$ docker run --name certain-pgbouncer --link some-postgres:postgres -p 6432:6432 -d kafka/pgbouncer
```

or you can manually configure via special environment variables as follows:

```bash
docker run \
  -e "POSTGRES_PORT=tcp://127.0.0.1:5432" \
  -e "POSTGRES_ENV_POSTGRES_USER=postgres" \
  -e "POSTGRES_ENV_POSTGRES_DB=postgres" \
  -e "POSTGRES_ENV_POSTGRES_PASSWORD=mysecretpassword" \
  -p 6432:6432/tcp \
  -d --restart=always kafka/pgbouncer
```

By default pgbouncer instance binds to: 0.0.0.0:6432. You can change that and other behavior by providing optional environment variables.

#### Example

```bash
docker run \
  -e "listen_addr=0.0.0.0" \
  -e "listen_port=6432" \
  -e "pool_mode=session" \
  -e "default_pool_size=20" \
  -e "max_client_conn=100" \
  -p 6432:6432/tcp \
  -d --restart=always kafka/pgbouncer
```

As you can mention this example contains default values of parameters from pgbouncer config.

Supported Docker versions
=========================

This image is officially supported on Docker version 1.10.3.

Support for older versions (down to 1.6) is provided on a best-effort basis.

Please see [the Docker installation documentation](https://docs.docker.com/installation/) for details on how to upgrade your Docker daemon.

User Feedback
=============

Issues
------

If you have any problems with or questions about this image, please contact through a [GitHub issue](https://github.com/ofkindness/docker-pgbouncer/issues).

Contributing
------------

You are invited to contribute new features, fixes, or updates, large or small; It is always thrilled to receive pull requests, and process them.

Before you start to code, it is recommend discussing your plans through a [GitHub issue](https://github.com/ofkindness/docker-pgbouncer/issues), especially for ambitious contributions.
