# Flink demo Display and Generator Helm chart

This is a Helm chart for [flink](https://github.com/dcos/demos/tree/master/flink/1.10) demo.

## Deploy

To deploy Frontend App run:
```bash
helm install --name flink-demo --namespace flink dlc/flink-demo
```

Check that pods are running:

```bash
kubectl -n flink get pods
NAME                                    READY     STATUS    RESTARTS   AGE
flink-demo-actor-555c6d9767-hflvb       1/1       Running   0          26s
flink-demo-generator-7d7785f566-gfzpd   1/1       Running   0          26s
```

Check Display logs:
```bash
kubectl logs -n flink flink-demo-actor-555c6d9767-hflvb
```

## Remove

The release can be cleaned up with helm:

```bash
helm delete --purge flink-demo
```
