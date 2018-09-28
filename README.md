#What is RabbitMQ?


~~~
RabbitMQ is a messaging broker or an open source message broker software that acts as an intermediary by accepting the message sent by the producer as a receiver. 

RabbitMQ is also called message-oriented middleware and it implements the Advanced Message Queuing Protocol (AMQP). 

Erlang programming language is used to write the RabbitMQ server. 

RabbitMQ is built on the Open Telecom Platform framework for clustering.
~~~


# What is a server in RabbitMQ?


~~~
The RabbitMQ server is a robust and scalable implementation of an AMQP broker. 

Running rabbitmq-server in the foreground displays a banner message, and reports on progress in the startup sequence, concluding with the message "broker running", indicating that the RabbitMQ broker has been started successfully.
~~~


# What is an exchange in RabbitMQ?


~~~
An exchange in RabbitMQ is a message routing agent that routes the message to different queues after accepting the same from the producer application. 

It routes the messages with the help of header attributes, bindings, and routing keys.
~~~


# What is “binding” and “routing key”?


~~~
A binding in rabbitMQ is a "link" that one sets up to bind a queue to an exchange.

The routing key is a message attribute. This is how the routing algorithm is formed - a message is sent to the queues whose binding key exactly matches the routing key of the message.
~~~


![Image ipa](https://github.com/NileshChandekar/rabbitmq/blob/master/hello-world-example-routing.png)


# What are the different types of exchanges in RabbitMQ?


~~~
There are primarily 4 types of exchanges in RabbitMQ

	Direct Exchange

	Topic Exchange

	Fanout Exchange

	Headers Exchange
~~~


![Image ipa](https://github.com/NileshChandekar/rabbitmq/blob/master/exchanges-topic-fanout-direct.png)



# What is direct Exchange in RabbitMQ ?

~~~
Direct Exchange is used for the purpose of delivering message to the queue whose binding key exactly matches to message routing key.
~~~
~~~
If no matching queue can be found for the message, the message will be silently dropped. RabbitMQ provides an AMQP extension known as the "Dead Letter Exchange" - the dead letter exchange provides functionality to capture messages that are not deliverable.
~~~

# What is fanout Exchange in RabbitMQ?

~~~
A fanout exchange in RabbitMQ routes messages to all of the queues that are bound to it and it completely ignores the routing key.
~~~



# Message Exchange 


![Image ipa](https://github.com/NileshChandekar/rabbitmq/blob/master/Screen%20Shot%202017-04-05%20at%204.12.40%20PM.png)


# How in Openstack ?


![Image ipa](https://github.com/NileshChandekar/rabbitmq/blob/master/openstackrabbitmq.png)



# Call trace 


![Image ipa](https://github.com/NileshChandekar/rabbitmq/blob/master/Ceilometer-multi-publish2.jpg)


# Mnesia 

# Use Cases ? How to ?
