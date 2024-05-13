# Use PostgreSQL 14 Alpine as base image
FROM postgres:15-alpine
LABEL maintainer="quangdinhvh2@gmail.com"

# Install dependencies, download and install pg_bigm, configure PostgreSQL, and clean up in a single RUN command to reduce layers and minimize image size
RUN apk --update --no-cache add --virtual build-dependencies \
    curl \
    make \
    gcc \
    musl-dev \
    postgresql-dev \
    icu-dev \
    # Download and install pg_bigm
    && cd /tmp \
    && curl -L -O https://github.com/pgbigm/pg_bigm/releases/download/v1.2-20200228/pg_bigm-1.2-20200228.tar.gz \
    && tar zxfv pg_bigm-1.2-20200228.tar.gz \
    && cd pg_bigm-1.2-20200228 \
    && make USE_PGXS=1 \
    && make USE_PGXS=1 install \
    # Configure PostgreSQL
    && echo "shared_preload_libraries = 'pg_bigm'" >> /usr/local/share/postgresql/postgresql.conf.sample \
    # Cleanup
    && cd / \
    && rm -fr /tmp/pg_bigm-1.2-20200228 \
    && apk del --purge build-dependencies

# Expose the PostgreSQL port
EXPOSE 5432
