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

