# DC/OS Labs Kubernetes Helm charts repository 

## Install Helm

Get the latest [Helm release](https://github.com/kubernetes/helm#install).


## Install Charts

You need to add this Chart repo to Helm:
```console
$ helm repo add dlc https://dcos-labs.github.io/charts/
$ helm repo update
```
Now you can then run `helm search dlc` to see the available charts.

## Deployment

Check each chart's README.md for instructions how to install it.
