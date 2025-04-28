# streamsets-redis-tls
This project provides an example of how to add TLS support for [Redis](https://redis.io/) within [IBM StreamSets](https://www.ibm.com/products/streamsets) using [stunnel](https://www.stunnel.org/). 

This workaround won't be necessary once StreamSets Engine v6.3 ships with native TLS suport for Redis. In the meantime, this example may come in handy, and also serves as an example of how to deploy [sidecar containers](https://kubernetes.io/docs/concepts/workloads/pods/sidecar-containers/) alongside StreamSets engines when deploying on Kubernetes.

Stunnel is a proxy that adds TLS encryption support for unencrypted clients. There are many examples online of using stunnel with Redis, such as [this one](https://redis.io/blog/stunnel-secure-redis-ssl/)  and [this one](https://www.digitalocean.com/community/tutorials/how-to-connect-to-managed-redis-over-tls-with-stunnel-and-redis-cli). With stunnel in place, StreamSets' Redis connectors connect to a stunnel server which handles interaction with the backend Redis server using TLS..  

### Deployment Options
For standalone StreamSets deployments, one can simply install stunnel on any available machine (including, for example, the same machine the StreamSets engine is on).

For Kubernetes-based deployments, one can deploy stunnel as a sidecar container alongside a StreamSets engine.  This is the approach described in this example.

### Step 1 - Create and publish a stunnel Docker image
Create and publish a Docker container with stunnel installed.  An example Dockerfile is [here](docker/Dockerfile).

### Step 2 - Same a stunnel conf file in a ConfigMap.
An example stunnel.conf file is [here](stunnel-conf/stunnel.conf)


# stunnel.conf
debug = 7
output = /var/log/stunnel4/stunnel.log
foreground = yes

pid = /var/run/stunnel4/redis-server.pid
setuid = stunnel4
setgid = stunnel4

[redis-server]
client = yes
accept = 6379
connect = crusher.onefoursix.com:6380
cert = /etc/stunnel/tls/client.crt
key = /etc/stunnel/tls/client.key
