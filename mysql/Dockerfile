# Use the official ClickHouse image as the base image
FROM yandex/clickhouse-server:latest

# Modify the ClickHouse configuration as needed
RUN sed -i 's|<!-- Maximum memory usage for processing single query, in bytes. -->|<allow_experimental_database_materialized_postgresql>1</allow_experimental_database_materialized_postgresql>|' /etc/clickhouse-server/users.xml

# Copy your configuration files into the container
COPY ports.xml /etc/clickhouse-server/config.d/
COPY run.sh /run.sh

USER root

# Make the entry point script executable
RUN chmod +x /run.sh

USER clickhouse

# Set the entry point for the container
ENTRYPOINT ["/run.sh"]

CMD ["clickhouse-server"]

# Expose the necessary ports
EXPOSE 8124 9000
