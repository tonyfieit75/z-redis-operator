
## Redis Operator for IBM LinuxOne on Red Hat OpenShift

This operator only supports versions of redis `=>6`.

## Architecture

<div align="center">
    <img src="./static/redis-operator-architecture.png">
</div>

## Purpose

There are multiple problems that people face while setting up redis setup on Kubernetes, specially cluster type setup. The purpose of creating this opperator is to provide an easy and production ready interface for redis setup that include best-practices, security controls, monitoring, and management.

## Supported Features

Here the features which are supported by this operator:-

- Redis cluster and standalone mode setup
- Redis cluster failover and recovery
- Inbuilt monitoring with redis exporter
- Password and password-less setup of redis
- TLS support for additional security layer
- Ipv4 and Ipv6 support for redis setup


## Getting Started

The configuration of Redis setup should be described in [CRD definitions](config/crd/bases). All the examples related to redis standalone and cluster setup can be found inside [example](./example) folder.

## Prerequisites

Redis operator requires a Kubernetes cluster of version `>=1.18.0`. If you have just started with Operators, it's highly recommended using the latest version of Kubernetes.

## Image Compatibility

The following table shows the compatibility between the Operator Version, Redis Image, Sentinel Image, and Exporter Image:

| Operator Version | Redis Image | Sentinel Image | Exporter Image |
|------------------|-------------|----------------|----------------|
| v0.15.0          | v7.0.12     | v7.0.12        | v1.48.0        |
| v0.15.0          | v7.0.11     | v7.0.11        | v1.48.0        |
| v0.14.0          | v7.0.7      | v7.0.7         | v1.48.0        |
| v0.13.0          | v6.2.5      | nil            | v1.48.0        |

## Quickstart

The setup can be done by using helm. If you want to see more example, please go through the [example](./example) folder.

But you can simply use the helm chart for installation.

```shell
# Add the helm chart
$ helm repo add z-helm https://tonyfieit75.github.io/helm-charts/
```

```shell
# Deploy the redis-operator
$ helm upgrade redis-operator z-helm/redis-operator \
  --install --create-namespace --namespace z-operators
```

After deployment, verify the installation of operator

```shell
helm test redis-operator --namespace z-operators
```

Creating redis cluster, standalone, replication and sentinel setup.

```shell
# Create redis cluster setup
$ helm upgrade redis-cluster z-helm/redis-cluster \
  --set redisCluster.clusterSize=3 --install \ 
  --namespace z-operators
```

```shell
# Create redis standalone setup
$ helm upgrade redis z-helm/redis \
  --install --namespace z-operators
```

```shell
# Create redis replication setup
$ helm upgrade redis-replication z-helm/replication \
  --install --namespace z-operators
```

```shell
# Create redis sentinel setup
$ helm upgrade redis-sentinel z-helm/sentinel \
  --install --namespace z-operators
```

If you want to customize the value file by yourself while initializing the helm command, the values files for reference are present [here](https://github.com/tonyfieit75/helm-charts/tree/main/charts/redis).

## Monitoring with Prometheus

To monitor redis performance we will be using prometheus. In any case, extra prometheus configuration will not be required because we will be using the Prometheus service discover pattern. For that we already have set these annotations:-

```yaml
  annotations:
    redis.opstreelabs.in: "true"
    prometheus.io/scrape: "true"
    prometheus.io/port: "9121"
```

