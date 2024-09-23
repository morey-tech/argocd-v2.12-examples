# What's New in Argo CD v2.12?

## 6. Consistent-Hashing Algorithm

> Implemented by [Akram Ben Aissi](https://github.com/akram) in [#16564](https://github.com/argoproj/argo-cd/pull/16564), closing [#16570](https://github.com/argoproj/argo-cd/issues/16570).

The `application-controller` for Argo CD is responsible for managing `Applications` and their resulting resources across all the clusters managed by an instance. To scale the controller, it must be sharded, meaning that there are multiple instances of the controller. Each is responsible for a sub-set of clusters or Applications.

There are a number of challenges the application controller sharding. Primarily in regarding to the distribution of load between shards and the rebalancing during changes to clusters. These challenges often lead to some shards handling most of the work while others sit idle with very little load.

The distribution of work between controllers is determined by the sharding algorithm. The existing algorithms, including the `legacy` method and the `round-robin` algorithm, have limitations maintaining optimal load distribution and minimizing unnecessary cluster-to-shard assignment changes.

In Argo CD 2.12, a [consistent hashing](https://en.wikipedia.org/wiki/Consistent_hashing) ([with bounded loads](https://research.googleblog.com/2017/04/consistent-hashing-with-bounded-loads.html)) sharding algorithm which aims to improve the efficiency and effectiveness of the Argo CD application controller shards, particularly when changing the number of shards, clusters, or applications. The new algorithm will provide the following benefits:

* **Reduced Cluster-to-Shard Assignment Changes**: Minimize the redistribution of cluster-to-shard assignments, especially when altering the number of shards, resulting in more stable and efficient operation.  
* **Dynamic Load Balancing**: The algorithm should dynamically adjust weights based on the load, ensuring that the shard with the fewest applications receives a higher weight during shard assignment, leading to improved load balancing.  
* **Optimized Resource Utilization**: The algorithm should contribute to better resource utilization by maintaining a balanced distribution of applications across shards and preventing excessive resource utilization on specific shards.

The new algorithm is considered an "alpha feature" and the project is actively seeking feedback from end-users. It can be enabled using the `consistent-hashing` value for the key `controller.sharding.algorithm` of the `argocd-cmd-params-cm` `configMap`.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: argocd-cmd-params-cm
  namespace: argocd
  labels:
    app.kubernetes.io/name: argocd-cm
    app.kubernetes.io/part-of: argocd
data:
  controller.sharding.algorithm: "consistent-hashing"
```
