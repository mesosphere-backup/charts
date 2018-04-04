# OS Detector demo Helm chart

This is a Helm chart for [os-detector](https://github.com/mesosphere/dcos-kubernetes-quickstart/blob/master/examples/os-detector/os-detector.md) demo.

## Deploy Casandra on DC/OS

Please follow [os-detector readme](https://github.com/mesosphere/dcos-kubernetes-quickstart/blob/master/examples/os-detector/os-detector.md#os-detector) how to install Cassandra on DC/OS, but skip the `kubectl apply ...` command, which is going to be replaced with Helm chart.

## Deploy Frontend on Kubernetes

### Web Relay Ingress Controller

[Web Relay](https://webhookrelay.com) allows to expose your Kubernetes services externally to Internet.

Create a free account at [Web Relay site](https://webhookrelay.com)

Install `relay` cli:

```bash
brew install webhookrelay/tap/relay
```

Install Web Relay Ingress Controller into your Kubernetes cluster:

```bash
relay ingress init --no-rbac
```

Check that pod is running:

```bash
kubectl -n webrelay-ingress get pod
NAME                       READY     STATUS    RESTARTS   AGE
webrelay-8c4d57749-mql9x   1/1       Running   0          1m
```

Create Web Relay tunnel:

```bash
relay tunnel create --group webrelay-ingress os-detector
```
*** Take a note of tunnel endpoint - e.g. 2p4ptkh9vutgm8tqavigja.webrelay.io ***


### Frontend App

To deploy Frontend App (do not forget to replace there `host` with webrelay.io tunnel name) run:
```bash
helm upgrade --install os-detector --namespace os-detector dlc/os-detector \
  --set ingress.enabled="true",ingress.host="2p4ptkh9vutgm8tqavigja.webrelay.io"
```

Check that pods are running:
```bash
kubectl -n os-detector get pods
NAME                          READY     STATUS    RESTARTS   AGE
os-detector-8d4d57cbf-g65ls   1/1       Running   0          59s
os-detector-8d4d57cbf-jznjm   1/1       Running   0          59s
os-detector-8d4d57cbf-r4n27   1/1       Running   0          59s
os-detector-8d4d57cbf-vfwfx   1/1       Running   0          59s
os-detector-8d4d57cbf-zgp42   1/1       Running   0          59s
```

### Testing access

Now you should be able to check os-detector at http://xxxxxx.webrelay.io/

## Cleanup

The os-detector release can be cleaned up with helm:

```bash
helm delete --purge os-detector
```

And the Webrelay ingress can be cleaned up with `relay` cli:

```bash
relay ingress reset
```
