# Beer demo Front-end Web app Helm chart

This is Front-end Web App Helm chart for [dcos-k8s-beer-demo](https://github.com/dcos/demos/tree/master/dcos-k8s-beer-demo/1.10).

## Deploy Frontend App without the ingress

To deploy Frontend App run:
```bash
helm install --name beer --namespace beer dlc/beer-service-web
```

Check that pods are running:

```bash
kubectl -n beer get pods
NAME                                                    READY     STATUS    RESTARTS   AGE
beer-beer-service-1676235277-s9ptv                      1/1       Running   0          2m
beer-beer-service-1676235277-tskrp                      1/1       Running   0          2m
```

Then to can access app locally run:
```bash
kubectl port-forward -n beer beer-beer-service-1676235277-tskrp 8080
```

Now you should be able to check beer at http://127.0.0.1:8080

## Deploy Frontend App with the ingress support

To deploy Frontend App run (do not forget to replace there with your domain_name):
```bash
helm upgrade --install beer --namespace beer dlc/beer-service \
    --set ingress.enabled="true",ingress.host="beer.mydomain.com"
```

Please install [Cloudflare Warp Ingress Controller chart](../cloudflare-warp-ingress) to expose your app externally.


## Remove

The release can be cleaned up with helm:

```bash
helm delete --purge beer
```
