FROM yandex/clickhouse-server:latest

RUN sed -i 's|<!-- Maximum memory usage for processing single query, in bytes. -->|<allow_experimental_database_materialized_postgresql>1</allow_experimental_database_materialized_postgresql>|' /etc/clickhouse-server/users.xml

COPY ports.xml /etc/clickhouse-server/config.d/

RUN apt-get update && apt-get install -y lsb-release wget gnupg \
    && echo "deb https://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list \
    && wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add - \
    && apt-get update && apt-get install -y postgresql

COPY init.sh /docker-entrypoint-initdb.d/

RUN service postgresql start && \
    su - postgres -c "createdb homeassistant" && \
    su - postgres -c "psql -c \"CREATE USER homeassistant WITH PASSWORD 'homeassistant';\"" && \
    su - postgres -c "psql -c \"GRANT ALL PRIVILEGES ON DATABASE homeassistant TO homeassistant;\""

EXPOSE 5432 8124 9000
