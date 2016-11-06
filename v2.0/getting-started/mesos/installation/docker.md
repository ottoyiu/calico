---
title: Installing Calico for the Docker Containerizer
---

This document provides the commands to download and run Calico
for use with the Docker Containerizer in Mesos.

## Prerequisite: Docker
Calico networks Docker tasks for Mesos with its libnetwork plugin. In order to
run the libnetwork plugin, the Docker daemon on each agent must be configured
with a cluster store.

If using etcd as a cluster store, for example, run the Docker daemon with the
following additional parameter:

```shell
    --cluster-store=etcd://<ETCD HOST>:<PORT>
```

Replacing `<ETCD HOST>:<PORT>` with the appropriate `hostname:port`
for your etcd cluster.

## Install and Run Calico
It is very easy to install Calico to use with the
Docker Containerizer.

1. On each Mesos Agents, download the `calicoctl` command-line tool:

```shell
curl -o /usr/bin/calicoctl -L https://github.com/projectcalico/calico-containers/releases/download/v1.0.0-beta/calicoctl
chmod a+x calicoctl
```

2. Launch the `calico/node` container.

For production deployments, we recommend running the
container as a service. Visit our guide on [running Calico
as a service]({{site.baseurl}}/{{page.version}}/usage/configuration/as-service) to learn how to do this.

For test environments that you would like to get up and running
quickly, you can launch the container with `calicoctl`:

```shell
sudo ETCD_ENDPOINTS=http://<ETCD HOST:PORT> ./calicoctl noderun
```

Again, be sure to set the ETCD_ENDPOINTS to the correct value for your etcd cluster.

3. Ensure calico's services are running by checking for the Calico container in Docker:

```shell
$ docker ps
CONTAINER ID        NAMES               IMAGE                           CREATED
f237fb21d357        calico-node         calico/node:v1.0.0-beta              3 seconds
```

4. Enable Docker Containerizer in Mesos.

By default, Mesos enables on the "Mesos" Containerizer. Be sure to also
enable the Docker Containerizer:

```shell
sh -c 'echo docker > /etc/mesos-slave/containerizers'
systemctl restart mesos-slave.service
```

## Next Steps

With Calico Installed, you're now ready to launch Calico-networked tasks. See the [Docker Containerizer Usage Guide]({{site.baseurl}}/{{page.version}}/getting-started/mesos/tutorials/docker) for information.