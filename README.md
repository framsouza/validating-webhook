### Validating Webhook hostpath

This is a Validationg Webhook Configuration that will denied the creation of pods that are using `/` to mount `hostPath` volumes. For more informations about the risk of using hostPath, please check this [doc page](https://kubernetes.io/docs/concepts/storage/volumes/#hostpath).

This is using cert-manager to generate the certs.

### Usage

1. `kind create cluster`
2. Install cert manager
  - `helm install cert-manager jetstack/cert-manager --namespace cert-manager --create-namespace --version v1.9.1 --set installCRDs=true`
3. `git clone git@github.com:framsouza/validating-webhook.git`
4. `kubectl create -f manifests/cert-manager.yaml`
5. `kubectl create -f manifests/validation.yaml`
6. `kubectl create -f manifests/webhook.yaml`
7. `kubectl create -f manifests/bad-pod.yaml`

  The command will try to spin up a pod that mounts `/`. The output is the following:
```
kubectl create -f manifests/bad-pod.yaml 
namespace/apps created
Error from server: error when creating "manifests/bad-pod.yaml": admission webhook "hostpah-kubernetes-webhook.acme.com" denied the request: pod contains "/" as hostPath
```

