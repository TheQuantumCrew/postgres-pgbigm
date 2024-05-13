## Custom PostgreSQL with pg_bigm Extension

This repository contains Dockerfiles to build custom PostgreSQL images enhanced with the `pg_bigm` extension. The Dockerfiles are organized by PostgreSQL version, each in its own directory.

## About pg_bigm

The `pg_bigm` extension allows you to create a bigram index for full text search capabilities in PostgreSQL. It is particularly suited for Japanese text search but works effectively for other languages as well. For more information, visit [pg_bigm&#39;s GitHub repository](https://github.com/pgbigm/pg_bigm).

### Directory Structure

```sh
├── 14-alpine
│   ├── Dockerfile
│   └── build.sh
├── 15-alpine
│   ├── Dockerfile
│   └── build.sh
└── README.md
```

Each version-specific directory contains a `Dockerfile` and a `build.sh` script for building the Docker image.

### Prerequisites

Before you begin, ensure you have Docker installed on your system. You can download and install Docker from [Docker&#39;s official website](https://www.docker.com/products/docker-desktop).

### Building the Docker Images (No need to build if you are using the images from Docker Hub)

To build a Docker image, navigate to the specific version directory you wish to build. For example, to build the PostgreSQL 14 with `pg_bigm` image, use the following commands:

```bash
cd 14-alpine
./build.sh
# or
source build.sh
```

Alternatively, you can use the `docker build` command directly:

```bash
docker build -t banhmysuawx/postgres-pgbigm:14-alpine .
```

This command builds the Docker image and tags it as `banhmysuawx/postgres-pgbigm:14-alpine`.

### Running the Docker Container

Once the image is built, you can run a container from it. Here's how to start a PostgreSQL container using the image tagged `banhmysuawx/postgres-pgbigm:14-alpine`:

```sh
# Create a directory to store the PostgreSQL data
mkdir -p ~/dev/postgres14-pgbigm

docker run --name my-custom-postgres \
  -e POSTGRES_USER=postgres \
  -e POSTGRES_PASSWORD=postgres \
  -v ~/dev/postgres14-pgbigm:/var/lib/postgresql/data \
  -p 5432:5432 \
  -d banhmysuawx/postgres-pgbigm:14-alpine #Change the image name to the one you built example banhmysuawx/postgres-pgbigm:15-alpine if you built the 15-alpine image
```

#### Parameters Explained:

* `--name my-custom-postgres`: Names your container for easier management.
* `-e POSTGRES_USER=postgres` and `-e POSTGRES_PASSWORD=postgres`: Sets the default username and password for PostgreSQL.
* `-v ~/dev/postgres14-pgbigm:/var/lib/postgresql/data`: Mounts the host directory `~/dev/postgres14-pgbigm` to the container's data directory. Adjust the path as necessary.
* `-p 5434:5432`: Maps port 5434 on the host to port 5432 in the container, allowing you to access PostgreSQL on port 5434 of your localhost.
* `-d`: Runs the container in detached mode, allowing the terminal to be used for other commands.

### Installing and Verifying pg_bigm Extension

Once your container is running, connect to your PostgreSQL instance:

```sh
docker exec -it my-custom-postgres psql -U postgres
```

Then, create the `pg_bigm` extension by running:
```sql
=# CREATE EXTENSION pg_bigm;
```

To verify the installation, check the list of installed extensions:

```sql
=# \dx pg_bigm
                                 List of installed extensions
  Name   | Version | Schema |                           Description                            
---------+---------+--------+------------------------------------------------------------------
 pg_bigm | 1.2     | public | text similarity measurement and index searching based on bigrams
```

### Additional Information

* The `build.sh` script is a convenience script to build the Docker images without having to remember the full `docker build` command.
* Modify the Dockerfile or environment variables as necessary to suit your specific requirements.

### Conclusion

This setup allows you to deploy PostgreSQL with the `pg_bigm` extension in a containerized environment, providing an easy and consistent way to manage your PostgreSQL instances across different environments.

## Acknowledgments

Special thanks to [Kazaoki Lab](https://github.com/kazaoki/postgres-bigm) and all the contributors of the original `pg_bigm` project.I have built upon their work to create Docker images for newer PostgreSQL versions.
