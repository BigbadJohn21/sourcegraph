# Using external databases with Sourcegraph

Sourcegraph by default provides its own PostgreSQL and Redis databases for data storage:

- PostgreSQL for storing long-term information, such as user information when using Sourcegraph's built-in authentication provider instead of an external one.
- Redis for storing short-term information, such as session information and cache data.

## Using your own PostgreSQL server

You can use your own PostgreSQL server with Sourcegraph if you wish. For example, you may prefer this if you already have existing backup infrastructure around your own PostgreSQL server.

Simply add the standard PostgreSQL environment variables to your `docker run` command and Sourcegraph will use that PostgreSQL server instead of its built-in one. For example:

```shell
docker run [...] \
  -e PGHOST=psql.mycompany.org \
  -e PGUSER=sourcegraph \
  -e PGPASSWORD=secret \
  -e PGDATABASE=sourcegraph \
  -e PGSSLMODE=disable \
  sourcegraph/server:2.12.2
```

Most standard PostgreSQL environment variables may be specified (`PGPORT`, etc). See http://www.postgresql.org/docs/current/static/libpq-envars.html for a full list.

> NOTE: On Mac/Windows, if trying to connect to a PostgreSQL server on the same host machine, remember that Sourcegraph is running inside a Docker container inside of the Docker virtual machine. You may need to specify your actual machine IP address and not `localhost` or `127.0.0.1` as that refers to the Docker VM itself.

## Using your own Redis server

Generally, there is no reason to do this as Sourcegraph only stores ephemeral cache and session data in Redis. However, if you want to use an external Redis server with Sourcegraph, you can:

Simply add the `REDIS_ENDPOINT` environment variable to your `docker run` command and Sourcegraph will use that Redis server instead of its built-in one. For example:

```
docker run [...] \
  -e REDIS_ENDPOINT=redis.mycompany.org:6379 \
  sourcegraph/server:2.12.2
```

> NOTE: On Mac/Windows, if trying to connect to a Redis server on the same host machine, remember that Sourcegraph is running inside a Docker container inside of the Docker virtual machine. You may need to specify your actual machine IP address and not `localhost` or `127.0.0.1` as that refers to the Docker VM itself.
