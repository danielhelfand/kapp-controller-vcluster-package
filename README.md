# kapp-controller-vcluster-package

This is an attempt to create a kapp-controller Package to deploy vclusters to a Kubernetes cluster's namespaces. 
I hope it's illustrative if this approach is interesting to you.

### Using this Example

1. Start up minikube

2. Install kapp-controller: https://carvel.dev/kapp-controller/docs/v0.32.0/install/

3. Deploy the Package to namespace you wish to use it in or to the kapp-controller global namespace to deploy it anywhere 
(i.e. `kapp-controller-packaging-global`)

```
kapp deploy -a vcluster-pkg -f vcluster-package.yml -n kapp-controller-packaging-global
```

4. Create the RBAC for the PackageInstall

```
kapp deploy -a rbac -f rbac/ -n NAMESPACE_WHERE_PACKAGEINSTALL_WILL_BE_CREATED
```

5. Deploy the PackageInstall to its destination namespace:

```
kapp deploy -a vcluster -f vcluster-pkgi.yml -f vcluster-vals.yml -n NAMESPACE_WHERE_PACKAGEINSTALL_WILL_BE_CREATED
```

6. Connect to the vcluster

```
$ kubectl get secret vc-my-vcluster -n NAMESPACE_WHERE_PACKAGEINSTALL_WILL_BE_CREATED --template={{.data.config}} | base64 -D > kubeconfig-vcluster.yml
$ kubectl port-forward my-vcluster-0 -n NAMESPACE_WHERE_PACKAGEINSTALL_WILL_BE_CREATED 8443
```

To run commands against the vcluster, you can use the kubeconfig-vclsuter.yml file like below:

```
kubectl get ns --kubeconfig=path/to/dir/kubeconfig-vcluster.yml
```
