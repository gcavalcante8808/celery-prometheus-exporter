# Celery Exporter

A simple Prometheus-compatible metrics exporter for Celery

## Introduction

This chart bootstraps a Celery Exporter deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

## Prerequisites

- Kubernetes 1.3+ with Beta APIs enabled
- Make 4.1+ and Bash (for installation through make)

## Installing using Helm

After cloning the repository, you need to provide at least the following attrs :

Parameter | Description | Default
--------- | ----------- | -------
`image.tag` | Celery Exporter container image tag | `1.6.0-celery3`
`broker_url` | AMQP/Redis address. Eg: redis://cache:6379/0 | 
`timezone` | Timezone of the celery queues. eg. America/Sao_Paulo
`queue_list` | Queue names split by comma and escaped.

To install the chart with the release name `my-release` and monitor the queues `default` and `zone1` on namespace `default` which is running under `America/Sao_Paulo` TZ:

```console
    helm upgrade --namespace default \
        --install --force my-release \
        --set broker_url=redis://cache:6379/0 \
        --set queue_list="default\,zone1" \
        --set timezone="America/Sao+Paulo" \
        .

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```console
$ helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.


## Configuration

The following table lists the configurable parameters of the Celery Exporter chart and their default values.

Parameter | Description | Default
--------- | ----------- | -------
`image.repository` | Celery Exporter container image repository | `zerok/celery-prometheus-exporter`
`image.tag` | Celery Exporter container image tag | `1.6.0-celery3`
`image.pullPolicy` | Celery Exporter container image pull policy | `IfNotPresent`
`server.service.type` | type of Celery Exporter service to create | `ClusterIP`
`broker_url` | AMQP/Redis address. Eg: redis://cache:6379/0 | 
`timezone` | Timezone of the celery queues. eg. America/Sao_Paulo
`queue_list` | Queue names split by comma and escaped.

## Features

The service created by the chart come with the following annotations:

```yaml
  annotations:
    prometheus.io/scrape: "true"
    prometheus.io/port: "8888"
```

If your prometheus is configured to pull service configuration from k8s using those annotations, 
the new metrics should be discovered and pulled automatically.

## Running Tests

There are just some smoke tests for the project. Before you can invoke them, you need to install the `Helm Tiller Plugin` with the following command:

````console
$ helm plugin install https://github.com/rimusz/helm-tiller
````

After that, you can run the tests throught the command `make test`.