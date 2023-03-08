# presto-event-stream
inspired by 
* https://github.com/IBM/trino-event-stream
* https://github.com/aws-samples/emr-presto-query-event-listener
* https://github.com/trinodb/trino/tree/master/plugin/trino-http-event-listener

## Install

TODO

## Configuration

Create new properties file `event-listener.properties` inside the `/etc/` directory:

```
event-listener.name=event-stream
bootstrap.servers=broker:9092
key.serializer=org.apache.kafka.common.serialization.StringSerializer
value.serializer=org.apache.kafka.common.serialization.StringSerializer
```

`Avro` formatter is added to serialize messages generated from QueryCreatedEvent, QueryCompletedEvent. 
Avro formatted messages would be read as String using the StringSerializer, then it will emit events to the Kafka topic `presto.event`.

## Post event analysis with Presto
We would use [Hudi/Deltastramer](https://hudi.apache.org/docs/hoodie_deltastreamer) to sink the kafka topic easily
```
--schemaprovider-class org.apache.hudi.utilities.schema.FilebasedSchemaProvider
--source-class org.apache.hudi.utilities.sources.JsonKafkaSource
--hoodie-conf bootstrap.servers=broker:9092
--hoodie-conf hoodie.deltastreamer.schemaprovider.source.schema.file=QueryCompletedEvent.avsc
```
