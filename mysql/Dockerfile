FROM yandex/clickhouse-server:latest

COPY run.sh /

RUN chmod a+x /run.sh

# Заменяем конфигурацию ClickHouse
RUN sed -i 's|<!-- Maximum memory usage for processing single query, in bytes. -->|<allow_experimental_database_materialized_postgresql>1</allow_experimental_database_materialized_postgresql>|' /etc/clickhouse-server/users.xml

# Копируем ports.xml
COPY ports.xml /etc/clickhouse-server/config.d/

# Экспонируем порты
EXPOSE 8124 9000

# Запускаем скрипт run.sh при запуске контейнера
CMD [ "/run.sh" ]
