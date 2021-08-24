# Katafygio

[Katafygio](https://github.com/bpineau/katafygio) discovers Kubernetes objects (deployments, services, ...), and continuously save them as YAML files in a git repository. This provides real time, continuous backups, and keeps detailed changes history.

## TL;DR;

```bash
$ helm install assets/helm-chart/katafygio/
```

## Introduction

This chart installs a [Katafygio](https://github.com/bpineau/katafygio) deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

## Prerequisites

- Kubernetes 1.8+

## Chart Details

If your backups are flooded by commits from uninteresting changes, you may filter out irrelevant objects using the `excludeKind`, `excludeObject`, `excludeNamespaces`, and `excludeHavingOwnerRef` options.

By default, the chart will dump (and version) the clusters content in /tmp/kf-dump (configurable with `localDir`).
This can be useful as is, to keep a local and ephemeral changes history. To benefit from long term, out of cluster, and centrally reachable persistence, you may provide the address of a remote git repository (with `gitUrl`), where all changes will be pushed.

## Installing the Chart

To install the chart with the release name `my-release`:

```bash
$ helm install --name my-release assets/helm-chart/katafygio/
```

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```console
$ helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration

The following table lists the configurable parameters of the Katafygio chart and their default values.

| Parameter               | Description                                                 | Default                              |
|-------------------------|-------------------------------------------------------------|--------------------------------------|
| `replicaCount`          | Desired number of pods (leave to `1` when using local PV)   | `1`                                  |
| `image.repository`      | Katafygio container image name                              | `bpineau/katafygio`                  |
| `image.tag`             | Katafygio container image tag                               | `v0.8.4`                             |
| `image.pullPolicy`      | Katafygio container image pull policy                       | `IfNotPresent`                       |
| `localDir`              | Container's local path where Katafygio will dump and commit | `/tmp/kf-dump`                       |
| `gitTimeout`            | Deadline for all git commands                               | `300s`                               |
| `gitUrl`                | Optional remote repository where changes will be pushed     | `nil`                                |
| `noGit`                 | Disable git versioning                                      | `false`                              |
| `filter`                | Label selector to dump only matched objects                 | `nil`                                |
| `healthcheckPort`       | The port Katafygio will listen for health checks requests   | `8080`                               |
| `excludeKind`           | Object kinds to ignore                                      | `{"replicaset","endpoints","event"}` |
| `excludeObject`         | Specific objects to ignore (eg. "configmap:default/foo")    | `nil`                                |
| `excludeNamespaces`     | List of regexps matching namespaces names to ignore         | `nil`                                |
| `excludeHavingOwnerRef` | Ignore all objects having an Owner Reference                | `false`                              |
| `rbac.create`           | Enable or disable RBAC roles and bindings                   | `true`                               |
| `rbac.apiVersion`       | RBAC API version                                            | `v1`                                 |
| `serviceAccount.create` | Whether a ServiceAccount should be created                  | `true`                               |
| `serviceAccount.name`   | Service account to be used                                  | `nil`                                |
| `resyncInterval`        | Seconds between full catch-up resyncs. 0 to disable         | `300`                                |
| `logLevel`              | Log verbosity (ie. info, warning, error)                    | `warning`                            |
| `logOutput`             | Logs destination (stdout, stderr or syslog)                 | `stdout`                             |
| `logServer`             | Syslog server address (eg. "rsyslog:514")                   | `nil`                                |
| `resources`             | CPU/Memory resource requests/limits                         | `{}`                                 |
| `tolerations`           | List of node taints to tolerate                             | `[]`                                 |
| `affinity`              | Node affinity for pod assignment                            | `{}`                                 |
| `nodeSelector`          | Node labels for pod assignment                              | `{}`                                 |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`.

Alternatively, a YAML file that specifies the values for the above parameters can be provided while installing the chart. For example,

```bash
$ helm install --name my-release -f values.yaml assets/helm-chart/katafygio/
```
> **Tip**: You can use the default [values.yaml](values.yaml)
