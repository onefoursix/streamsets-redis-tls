# streamsets-redis-tls
This project provides an example of how to add TLS support for [Redis](https://redis.io/) within [IBM StreamSets](https://www.ibm.com/products/streamsets) pipelines.  This won't be necessary once StreamSets Engine v6.3 ships with native TLS suport for Redis. In the meantime, this example may come in handy, and also serves as an example of how to deploy [sidecar containers](https://kubernetes.io/docs/concepts/workloads/pods/sidecar-containers/) alongside StreamSets engines when deploying on Kubernetes.

## What's the problem?
The current version of StreamSets' Redis connector does not support TLS, which blocks connectivity to Redis servers configured to use TLS.

## What's the solution?
StreamSets engine v6.3, to be released shortly, will add support for Redis TLS.

## What's the workaround in the meantime?
In the meantime, one can use [stunnel](https://www.stunnel.org/) as a proxy that adds TLS encryption support for unencrypted clients. There are many examples online of using stunnel with Redis, such as [this one](https://redis.io/blog/stunnel-secure-redis-ssl/)  and [this one](https://www.digitalocean.com/community/tutorials/how-to-connect-to-managed-redis-over-tls-with-stunnel-and-redis-cli). With stunnel in place, StreamSets Redis connectors would connect to a stunnel server which would interact with the backend Redis server using TLS.

For standalone StreamSets deployments, one can simply install stunnel on any available machine (including, for example, the same machine the StreamSets engine is on).  

When deploying StreamSets engines on Kubernetes, one can deploy stunnel as a sidecar container alongside StreamSets engines.  This is the approach described in this example.

## Step 1 - Create a stunnel Docker image
Create and publish a Docker container with stunnel installed.  An example Dockerfile is [here](docker/Dockerfile)
