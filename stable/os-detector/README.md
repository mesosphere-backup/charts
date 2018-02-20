# OS Detector demo Helm chart

This is a Helm chart for [os-detector](https://github.com/mesosphere/dcos-kubernetes-quickstart/blob/master/examples/os-detector/os-detector.md) demo.

## Deploy Casandra on DC/OS

Please follow [os-detector readme](https://github.com/mesosphere/dcos-kubernetes-quickstart/blob/master/examples/os-detector/os-detector.md#os-detector) how to install Cassandra on DC/OS, but skip the `kubectl apply ...` command, which is going to be replaced with Helm chart.

## Deploy Frontend on Kubernetes

### Frontend App

To deploy Frontend App (do not forget to replace there with your `domain_name`) run:
```bash
helm upgrade --install os-detector --namespace os-detector dlc/os-detector \
  --set ingress.enabled="true",ingress.host="os-detector.mydomain.com"
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

### Cloudflare Warp Ingress Controller

The Cloudflare Warp Ingress Controller makes connections between a Kubernetes service and the Cloudflare edge, exposing an application in your cluster to the internet at a hostname of your choice. A quick description of the details can be found at https://warp.cloudflare.com/quickstart/.

**Note:** Before installing Cloudflare Warp you need to obtain Cloudflare credentials for your domain zone.
The credentials are obtained by logging in to https://www.cloudflare.com/a/warp, selecting the zone where you will be publishing your services, and saving the file local folder.

To deploy Cloudflare Warp Ingress Controller [chart](https://github.com/dcos-labs/charts/tree/master/stable/cloudflare-warp-ingress) run:
```bash
helm upgrade --install os-detector-ingress --namespace os-detector dlc/cloudflare-warp-ingress --set cert=$(cat cloudflare-warp.pem | base64)
```

Check that pods are running:
```bash
kubectl -n os-detector get pods
NAME                                                          READY     STATUS    RESTARTS   AGE
os-detector-8d4d57cbf-g65ls                                   1/1       Running   0          2m
os-detector-8d4d57cbf-jznjm                                   1/1       Running   0          2m
os-detector-8d4d57cbf-r4n27                                   1/1       Running   0          2m
os-detector-8d4d57cbf-vfwfx                                   1/1       Running   0          2m
os-detector-8d4d57cbf-zgp42                                   1/1       Running   0          2m
os-detector-ingress-cloudflare-warp-ingress-cd84b466b-fnzw4   1/1       Running   0          11s
```

### Testing access

Now you should be able to check os-detector at https://os-detector.mydomain.com/

## Remove

The release can be cleaned up with helm:

```bash
helm delete --purge os-detector-ingress
helm delete --purge os-detector
```
