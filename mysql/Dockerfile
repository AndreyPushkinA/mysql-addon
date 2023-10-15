FROM ghcr.io/hassio-addons/debian-base/amd64:7.1.0

RUN apt-get update && \
    apt-get install -y apt-transport-https ca-certificates dirmngr

RUN mkdir -p /root/.gnupg && chmod 700 /root/.gnupg

RUN apt-get install -y sudo

RUN GNUPGHOME=$(mktemp -d) && \
    gpg --batch --no-default-keyring --keyring /usr/share/keyrings/clickhouse-keyring.gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 8919F6BD2B48D754 && \
    rm -r "$GNUPGHOME"

RUN chmod +r /usr/share/keyrings/clickhouse-keyring.gpg && \
    echo "deb [signed-by=/usr/share/keyrings/clickhouse-keyring.gpg] https://packages.clickhouse.com/deb stable main" > /etc/apt/sources.list.d/clickhouse.list && \
    apt-get update && \
    apt-get install -y clickhouse-server clickhouse-client

RUN sed -i 's|<!-- Maximum memory usage for processing single query, in bytes. -->|<allow_experimental_database_materialized_postgresql>1</allow_experimental_database_materialized_postgresql>|' /etc/clickhouse-server/users.xml

COPY ports.xml /etc/clickhouse-server/config.d/

RUN apt-get update && apt-get install -y lsb-release wget gnupg \
    && echo "deb https://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list \
    && wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add - \
    && apt-get update && apt-get install -y postgresql

COPY init.sh /docker-entrypoint-initdb.d/

EXPOSE 5432 8124 9000

RUN service postgresql start && \
    su - postgres -c "createdb homeassistant" && \
    su - postgres -c "psql -c \"CREATE USER homeassistant WITH PASSWORD 'homeassistant';\"" && \
    su - postgres -c "psql -c \"GRANT ALL PRIVILEGES ON DATABASE homeassistant TO homeassistant;\""
