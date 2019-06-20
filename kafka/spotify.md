# Spotify kafka

- Run
> docker run -d -p 2181:2181 -p 9092:9092 --env ADVERTISED_HOST=localhost --env ADVERTISED_PORT=9092 spotify/kafka

- Produce
> /opt/kafka_2.11-0.10.1.0/bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test

- Consume
> /opt/kafka_2.11-0.10.1.0/bin/kafka-console-consumer.sh --zookeeper localhost:2181 --topic test