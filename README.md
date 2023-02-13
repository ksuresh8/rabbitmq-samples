# rabbitmq-samples
rabbitmq-samples

# rabbitmq cluster with HQ
1. docker network create --subnet=192.168.0.0/16 cluster-network

C:\Users\admin>docker network create --subnet=192.168.0.0/16 cluster-network
8008cd9f3abebb11a57cdb4b067bd0e9c3869105b130bc2d516ae72189b1b945

2. create cluster nodes using two docker containters, which run RabbitMQ using the below commands
docker run --detach --hostname node1.rabbit --net cluster-network --ip 192.168.0.10 --name rabbitNode1 --add-host node2.rabbit:192.168.0.11 --publish "4369:4369" --publish "5672:5672"  --publish "15672:15672" --publish "25672:25672" --publish "35672:35672" --env "RABBITMQ_USE_LONGNAME=true" --env "RABBITMQ_ERLANG_COOKIE=cookie" rabbitmq:3-management

C:\Users\admin>docker run --detach --hostname node1.rabbit --net cluster-network --ip 192.168.0.10 --add-host node2.rabbit:192.168.0.11 --publish "4369:4369" --publish "5672:5672"  --publish "15672:15672" --publish "25672:25672" --publish "35672:35672" --env "RABBITMQ_USE_LONGNAME=true" --env "RABBITMQ_ERLANG_COOKIE=cookie" rabbitmq:3-management
Unable to find image 'rabbitmq:3-management' locally
3-management: Pulling from library/rabbitmq
7608715873ec: Pull complete
8147f1b064ec: Pull complete
b49348aba7cf: Pull complete
96fb4c28b2c1: Pull complete
626e07084b41: Pull complete
73b172b466f8: Pull complete
f9a647254aa1: Pull complete
9cc483b3e58b: Pull complete
e26d864f102c: Pull complete
f8ea54e7b0b8: Pull complete
c66e903e6c2d: Pull complete
Digest: sha256:53b9538d5fd25ecc13a8adeb2b7f21e73c2a22230f73ab030d10f1f51329a1a3
Status: Downloaded newer image for rabbitmq:3-management
dc3f09f71ae0f3a9e7dad32bd9d862a6e26fe1925d5de3b65a291d1c81d916ed

docker run --detach --hostname node2.rabbit --net cluster-network --ip 192.168.0.11 --name rabbitNode2 --add-host node1.rabbit:192.168.0.10 --publish "4370:4369" --publish "5673:5672"  --publish "15673:15672" --publish "25673:25672" --publish "35673:35672" --env "RABBITMQ_USE_LONGNAME=true" --env "RABBITMQ_ERLANG_COOKIE=cookie" rabbitmq:3-management

C:\Users\admin>docker run --detach --hostname node2.rabbit --net cluster-network --ip 192.168.0.11 --name rabbitNode2 --add-host node1.rabbit:192.168.0.10 --publish "4370:4369" --publish "5673:5672"  --publish "15673:15672" --publish "25673:25672" --publish "35673:35672" --env "RABBITMQ_USE_LONGNAME=true" --env "RABBITMQ_ERLANG_COOKIE=cookie" rabbitmq:3-management
9f88391f3c174c1238d1b8ab5014fa397d8f45af16aadbab4e87deb917aa2bf2

** Delete containter in case of issues.
C:\Users\admin>docker container rm rabbitNode2
rabbitNode2


