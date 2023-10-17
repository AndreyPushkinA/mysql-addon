FROM yandex/clickhouse-server:latest

RUN sed -i 's|<!-- Maximum memory usage for processing single query, in bytes. -->|<allow_experimental_database_materialized_postgresql>1</allow_experimental_database_materialized_postgresql>|' /etc/clickhouse-server/users.xml

COPY ports.xml /etc/clickhouse-server/config.d/

COPY run.sh /
RUN chmod a+x /run.sh

RUN /run.sh

EXPOSE 8124 9001
