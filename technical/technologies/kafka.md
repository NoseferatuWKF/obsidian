# Architecture
```
										BROKER
								-----------------------
	 PRODUCER (without key)---> |        TOPIC        |
					|			|---------------------|
			  (with key)------> |      PARTITION      |
								||-------------------||
								||   EVENT | EVENT   || --> CONSUMER
								||-------------------||
								|      PARTITION      |
								||-------------------||
								||   EVENT | EVENT   || --> CONSUMER
								||-------------------||
								|---------------------|
								-----------------------
```

>[!info]
topic: event database
event: data payload
partition: logically separated storage unit for topic
broker: topic server
kafka connect: client and server connector
producer: client that publishes events
consumer: client that subscribes events

# Kafka API

admin API
producer API
consumer API
kafka streams API
kafka connect API