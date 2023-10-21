FROM ubuntu:latest

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install -y apt-transport-https ca-certificates dirmngr sudo && \
    apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 8919F6BD2B48D754 && \
    echo "deb https://packages.clickhouse.com/deb stable main" | tee /etc/apt/sources.list.d/clickhouse.list && \
    apt-get update

RUN apt-get install -y clickhouse-server clickhouse-client

RUN sed -i 's|<!-- Maximum memory usage for processing single query, in bytes. -->|<allow_experimental_database_materialized_postgresql>1</allow_experimental_database_materialized_postgresql>|' /etc/clickhouse-server/users.xml

COPY ports.xml /etc/clickhouse-server/config.d/

CMD ["service", "clickhouse-server", "start"]

EXPOSE 8124 9000
