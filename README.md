# rabbitmq-samples
rabbitmq-samples

# rabbitmq cluster with HQ
## 1. docker network create --subnet=192.168.0.0/16 cluster-network

C:\Users\admin>

    docker network create --subnet=192.168.0.0/16 cluster-network
    8008cd9f3abebb11a57cdb4b067bd0e9c3869105b130bc2d516ae72189b1b945

## 2. create cluster nodes using two docker containters, which run RabbitMQ using the below commands

    docker run --detach --hostname node1.rabbit --net cluster-network --ip 192.168.0.10 --name rabbitNode1 --add-host node2.rabbit:192.168.0.11 --publish "4369:4369" --publish "5672:5672"  --publish "15672:15672" --publish "25672:25672" --publish "35672:35672" --env "RABBITMQ_USE_LONGNAME=true" --env "RABBITMQ_ERLANG_COOKIE=cookie" rabbitmq:3-management

    docker run --detach --hostname node2.rabbit --net cluster-network --ip 192.168.0.11 --name rabbitNode2 --add-host node1.rabbit:192.168.0.10 --publish "4370:4369" --publish "5673:5672"  --publish "15673:15672" --publish "25673:25672" --publish "35673:35672" --env "RABBITMQ_USE_LONGNAME=true" --env "RABBITMQ_ERLANG_COOKIE=cookie" rabbitmq:3-management

*Delete containter in case of issues.
C:\Users\admin>docker container rm rabbitNode2
rabbitNode2* 


## 3. To join the two nodes in the same cluster, ensure that the specific ports are accessible. 

 - 4369 EPDM: It is a service for peer discovery used by nodes and
   RabbitMQ CLI tools. Peer discovery locates nodes or peers for data
   communication in a peer-to-peer network.
 - 5672: It is a port used by AMQP protocol.
 - 25672: It is used for communication between nodes and CLI tools.
 - 35672-35682: It is used by CLI tools for communication with nodes.
 - 15672: It is used by the management UI for instances.
## 4. Stop the execution of RabbitMQ on the RabbitMQ2 node.

    docker exec rabbitNode2 rabbitmqctl stop_app
Then join the two nodes.

    docker exec rabbitNode2 rabbitmqctl join_cluster rabbit@node1.rabbit
Restart the application.

    docker exec rabbitNode2 rabbitmqctl start_app

## 5. Mirroring nodes

    rabbitmqctl set_policy ha-all "." '{"ha-mode":"all", "ha-sync-mode":"automatic"}' --apply-to all
