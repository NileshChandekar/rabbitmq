# What is RabbitMQ?


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

# Configuration :- 


~~~
[root@overcloud-controller-0 ~]# ls -lhrt /etc/rabbitmq/
total 12K
-rw-r--r--. 1 root root  23 Aug  8 04:11 rabbitmqadmin.conf
drwxr-xr-x. 2 root root   6 Aug  8 04:11 ssl
-rw-r--r--. 1 root root 905 Aug  8 04:11 rabbitmq.config
-rw-r--r--. 1 root root 291 Aug  8 04:11 rabbitmq-env.conf
[root@overcloud-controller-0 ~]# 
~~~

~~~
[root@overcloud-controller-0 ~]# ls -lhrt /var/log/rabbitmq/
total 1.5M
-rw-r-----. 1 rabbitmq rabbitmq 151K Sep 25 09:46 rabbit@overcloud-controller-0.log-20180812.gz
-rw-r-----. 1 rabbitmq rabbitmq 1.3M Sep 25 09:46 rabbit@overcloud-controller-0-sasl.log-20180926
-rw-r-----. 1 root     root        0 Sep 25 09:46 startup_err
-rw-r-----. 1 root     root      377 Sep 25 09:46 startup_log
-rw-r-----. 1 rabbitmq rabbitmq    0 Sep 26 03:23 rabbit@overcloud-controller-0-sasl.log
-rw-r-----. 1 rabbitmq rabbitmq    0 Sep 26 03:23 rabbit@overcloud-controller-0.log
-rw-r-----. 1 rabbitmq rabbitmq 117K Sep 28 10:36 rabbit@overcloud-controller-0.log-20180926
[root@overcloud-controller-0 ~]# 
~~~

~~~
[root@overcloud-controller-0 ~]# ls -lhrt /var/lib/rabbitmq/mnesia/
total 4.0K
drwxr-x--x. 2 rabbitmq rabbitmq    6 Sep 25 09:46 rabbit@overcloud-controller-0-plugins-expand
drwxr-x--x. 4 rabbitmq rabbitmq 4.0K Sep 25 09:49 rabbit@overcloud-controller-0
[root@overcloud-controller-0 ~]#
~~~

~~~
[root@overcloud-controller-0 ~]# egrep -v "^#|^$" /etc/rabbitmq/rabbitmqadmin.conf 
[default]
port = 15672
[root@overcloud-controller-0 ~]# 
~~~

~~~
[root@overcloud-controller-0 ~]# egrep -v "^#|^$" /etc/rabbitmq/rabbitmq.config 
% This file managed by Puppet
% Template Path: rabbitmq/templates/rabbitmq.config
[
  {rabbit, [
    {tcp_listen_options,
         [binary,
         {packet,        raw},
         {reuseaddr,     true},
         {backlog,       128},
         {nodelay,       true},
         {linger,        {true, 0}},
         {exit_on_close, false}]
    },
    {cluster_partition_handling, pause_minority},
    {loopback_users, []},
    {queue_master_locator, <<"min-masters">>},
    {tcp_listen_options, [binary, {packet, raw}, {reuseaddr, true}, {backlog, 128}, {nodelay, true}, {exit_on_close, false}, {keepalive, true}]},
    {default_user, <<"guest">>},
    {default_pass, <<"G68QE3cWRR4t4pdp2d6qFRnPw">>}
  ]},
  {kernel, [
    {inet_dist_listen_max, 25672},
    {inet_dist_listen_min, 25672}
  ]}
,
  {rabbitmq_management, [
    {listener, [
      {port, 15672}
      ,{ip, "192.168.24.16"}
    ]}
  ]}
].
% EOF
~~~

~~~
[root@overcloud-controller-0 ~]# egrep -v "^#|^$" /etc/rabbitmq/rabbitmq-env.conf 
NODE_IP_ADDRESS=192.168.24.16
NODE_PORT=5672
RABBITMQ_NODENAME=rabbit@overcloud-controller-0
RABBITMQ_SERVER_ERL_ARGS="+K true +P 1048576 -kernel inet_default_connect_options [{nodelay,true},{raw,6,18,<<5000:64/native>>}] -kernel inet_default_listen_options [{raw,6,18,<<5000:64/native>>}]"
[root@overcloud-controller-0 ~]# 
~~~



# Mnesia 

~~~
[root@overcloud-controller-0 ~]# ls -lhrt /var/lib/rabbitmq/mnesia/
total 4.0K
drwxr-x--x. 2 rabbitmq rabbitmq    6 Sep 25 09:46 rabbit@overcloud-controller-0-plugins-expand
drwxr-x--x. 4 rabbitmq rabbitmq 4.0K Sep 25 09:49 rabbit@overcloud-controller-0
[root@overcloud-controller-0 ~]#
~~~


~~~
To fix rabbitmq can you first do :- 

pcs resource disable rabbitmq on one controller. 

Then go to controller and Do 

ps aux|grep rabbit  ;  then 

pkill rabbitmq; 

kill -9 for any remaining rabbitmq processes. 
~~~

Remove your mnesia  and then 

~~~
re-enable the service by 

pcs resource enable rabbitmq 

pacemaker will try to fresh start the services

wait about 2 minutes ; then 

do pcs resource cleanup to finish the job. Finally check 

pcs status |grep -A1 rabbit to see if it's correctly re-started.
~~~

Begind the scene 

~~~
This can be happened when - 
	a) client request RabbitMQ
	b) RabbitMQ request to Server 
	c) Server process the request and produces the response. 
	d) server send response messages to rabbitmq
	e) Finally  rabbitmq response to the Client. 
~~~

So in the above call when the process gets stuck on any step from b to e, client gets a "Timed out waiting for RPC response" execption. 

So to diagnosis this we need to check the queues :-

~~~
# rabbitmqctl list_queues
# rabbitmqctl list_queues name consumers name
# rabbitmqctl list_queues name consumers messages | grep -vw "0$"
~~~

If a queue has a consumers, but also has a messages are accumulating there, It means that the corrsoponding service can not process messages in time or got stuck in a deadlock. 


Check RabbitMQ for integrity 
~~~
# rabbitmqctl cluster_status
~~~

If the above command returs all the nodes in the cluster, You might find that your cluster is partitioned. 
Partition is a good reason for some messages to get stuck in queues. 


Restart `rabbitmq` services help in this situation. This will release the stuck messgaes, 


# Use Cases ? How to ? Configurations ?

~~~
$ rabbitmq-server -detached
$ rabbitmqctl stop_app
$ rabbitmqctl reset
$ rabbitmqctl join_cluster rabbit@overcloud-controller-0
$ rabbitmqctl start_app

$ rabbitmqctl cluster_status
# rabbitmqctl eval "rabbit_diagnostics:maybe_stuck()."
# rabbitmqctl status |grep -i file -A4
# rabbitmqctl report
# rabbitmqctl list_queues name consumers messages | grep -vw "0$"

# sos_commands/rabbitmq/rabbitmqctl_cluster_status 
~~~



