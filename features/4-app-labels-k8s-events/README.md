# What's New in Argo CD v2.12?

## 4. Application Labels on Kubernetes Events

> Implemented by [Siddhesh Ghadi](https://github.com/svghadi) in [#18160](https://github.com/argoproj/argo-cd/pull/18160), closing [#11381](https://github.com/argoproj/argo-cd/issues/11381).

Labels on Kubernetes resources are often used to differentiate between teams, environments, cost centers, and services. The labels are then used to for routing of messages and alerts to appropriate channels or teams.

For Argo CD, `Applications` produce Kubernetes events when performing various operations, like initiating a sync or changes in health status. Prior to Argo CD 2.12, the events produced from `Applications` where tied to the Application resource but didn't include any of the metadata making it difficult to filter or route the events to the appropriate destination without creating fragile and complex logic to parse the team and environment from the Application name (if available at all).

As of Argo CD 2.12, labels on `Applications` or their `AppProject` defined in the (optional) `resource.includeEventLabelKeys` key (a comma-separated list of `metadata.labels` keys) of the `argocd-cm` `ConfigMap` can be added by the controller to Kubernetes events generated for `Applications`. The labels then establish an easy link between the event and the application, allowing for filtering and routing.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: argocd-cm
  namespace: argocd
  labels:
    app.kubernetes.io/name: argocd-cm
    app.kubernetes.io/part-of: argocd
data:
  # An optional comma-separated list of metadata.labels keys to add to Kubernetes events generated for Applications.
  resource.includeEventLabelKeys: team,env*
  # An optional comma-separated list of metadata.labels keys to exclude from Kubernetes events generated for Applications. Supports wildcards.
  resource.excludeEventLabelKeys: environment,bu
```

In case of conflict between labels on the Application and AppProject, the Application label values are prioritized. The `resource.excludeEventLabelKeys` key can be used to exclude any labels on `Applications` or `AppProjects` from events.
