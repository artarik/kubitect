<div markdown="1" class="text-center">
# K3s cluster
</div>

<div markdown="1" class="text-justify">

This example demonstrates how to deploy a Kubernetes cluster with Kubitect using the `k3s` manager instead of the default `kubespray` manager.
It uses a simple topology with one control plane node and one worker node.

!!! warning "Warning"

    K3s support is newer than the default Kubespray flow.
    In addition, the `k3s` manager currently supports only the `flannel` network plugin.

## Step 1: Create the configuration

To initialize a K3s cluster, set `kubernetes.manager` to `k3s` and `kubernetes.networkPlugin` to `flannel`.

```yaml title="k3s-cluster.yaml"
kubernetes:
  manager: k3s
  networkPlugin: flannel
```

??? abstract "Final cluster configuration <i class="click-tip"></i>"

    ```yaml title="k3s-cluster.yaml"
    hosts:
      - name: localhost
        connection:
          type: local

    cluster:
      name: k3s-cluster
      network:
        mode: nat
        cidr: 192.168.113.0/24
      nodeTemplate:
        user: k8s
        updateOnBoot: true
        ssh:
          addToKnownHosts: true
        os:
          distro: debian12
      nodes:
        master:
          instances:
            - id: 1
              ip: 192.168.113.10
        worker:
          instances:
            - id: 1
              ip: 192.168.113.21

    kubernetes:
      manager: k3s
      version: v1.33.5
      networkPlugin: flannel
      other:
        mergeKubeconfig: true
    ```

## Step 2: Apply the configuration

To deploy the cluster, apply the configuration file:

```sh
kubitect apply --config k3s-cluster.yaml
```

## Step 3: Access the kubeconfig

If `mergeKubeconfig` is enabled, the resulting context is merged into `~/.kube/config`.
Otherwise, you can export the kubeconfig explicitly:

```sh
kubitect export kubeconfig --cluster k3s-cluster
```

</div>
