# What's New in Argo CD v2.12?

## 3. Application Project Repository Credentials

> Implemented by [Blake Pettersson](https://github.com/blakepettersson) in [#18388](https://github.com/argoproj/argo-cd/pull/18388), closing [#9581](https://github.com/argoproj/argo-cd/issues/9581) and [#17897](https://github.com/argoproj/argo-cd/issues/17897). Original proposal added in [#18290](https://github.com/argoproj/argo-cd/pull/18290)

In a self-service multi-tenant environment, teams in separate AppProjects may want to use the same repositories. Using RBAC in AppProjects, other teams can be prevented from having access to each other's repository credentials.

However, prior to Argo CD 2.12, when there are multiple repository credentials referencing the same repository URL, Argo CD will use the first credential found matching the URL given. Which means if both Team A and Team B have credentials for Repo Y, Team B may end up using Team A's credentials. The workaround is have a credential shared between teams which means they reply on an Argo CD admin to add it for them and less fine-grained access control to repositories.

As of Argo CD 2.12, the repository credential lookup would attempt to find the first `repository` secret which matches the `project` and repository URL of the requesting application. If there are none matching the requested `project`, it will fall back to returning the first unscoped credential (i.e, the first credential with an empty `project` parameter).

Notably, the a repository credential `Secret` now accepts to the `project` field to facilitate this functionality.

With different sets of credentials, there is a potential for `AppProject`s to have fetch different manifests. To mitigate unintended access and conflicts, the `repo-server` will check out a separate copy of the repository per `AppProject`.

This enhancement will enable self-service capabilities for dev teams while preventing inadvertently exposing repository credentials belonging to other `AppProjects`.

```yaml
# This would only be available to Applications in the the `default` AppProject.
apiVersion: v1
kind: Secret
metadata:
  labels:
    argocd.argoproj.io/secret-type: repository
  name: repo-3844139773
  namespace: argocd
type: StringData
data:
  project: default
  type: git
  url: https://github.com/morey-tech/argo-cd-helm-values-demo
---
# This one would be the default for any AppProject without a scoped credential.
apiVersion: v1
kind: Secret
metadata:
  labels:
    argocd.argoproj.io/secret-type: repository
  name: repo-987442240
  namespace: argocd
type: StringData
data:
  type: git
  url: https://github.com/morey-tech/argo-cd-helm-values-demo
```
