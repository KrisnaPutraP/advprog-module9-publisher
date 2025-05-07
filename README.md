>How much data your publisher program will send to the message broker in one
run?

The publisher calls `publish_event` five times, so in one run it sends 5 separate AMQP messages. Each message’s body is the Borsh‐serialized `UserCreatedEventMessage`.

>The url of: “amqp://guest:guest@localhost:5672” is the same as in the subscriber
program, what does it mean?

That URL is just the connection string for the AMQP broker (in this case RabbitMQ), so by using the same string in both publisher and subscriber, we're telling both programs to:

- Speak AMQP (`amqp://`)
- Authenticate as user `guest` with password `guest`
- Connect to the broker running on `localhost`
- Use TCP port `5672` (the default AMQP port)

In other words, both publisher and subscriber are pointing at the same broker instance. That way, the publisher can send messages into exchanges/queues on that broker, and the subscriber can pull those very same messages back out.

## RabbitMQ

### Running RabbitMQ as message broker

![rabbitmq](images/Running%20RabbitMQ.png)

### Sending and Processing Event

![sspublisher](images/sspublisher.png)
![sssubscriber](images/sssubscriber.png)

What happened is, when the message broker or RabbitMQ is running and we execute the `Subscriber` and `Publisher` programs (using `cargo run`), the `Publisher` sends data to the message broker, and the `Subscriber` receives that data. In the illustration above, we can see that the Publisher sends the data once to the message broker, and the `Subscriber` receives it.

### Monitoring chart based on publisher

![sschart](images/sschart.png)

In the image above, I repeatedly ran the Publisher multiple times to significantly increase the message rates. Based on what I did, I understand that the message rates will rise when the Publisher sends data to the message broker frequently. If the message rates are high, it means the message broker is receiving a large volume of data from the Publisher, creating the pattern shown in the chart.
