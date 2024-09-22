# What's New in Argo CD v2.12?

## 2. Multi-Source Application Rollbacks

> Introduced by [Jorge Turrado](https://github.com/JorTurFer) in [#14124](https://github.com/argoproj/argo-cd/pull/14124) closing [#12580](https://github.com/argoproj/argo-cd/issues/12580). Support extended in [#18615](https://github.com/argoproj/argo-cd/pull/18615).

A key feature of Argo CD is the ability to rollback an Application to a previous revision. Despite rollbacks from the UI technically being a GitOps anti-pattern, it's incredible handy for fast rollbacks to mitigate a problem until a proper roll-forward (making the change in git to create a new revision) can be done.

When the initial support for multiple sources for `Applications` was [added in Argo CD v2.6](https://youtu.be/2VF2x72dZsQ), there were several places in the CLI and the UI where support hadn't been added, including rollbacks.

Check out a [full demo](https://www.youtube.com/watch?v=MlAWr8bVr0I&t=733s) of using multiple sources to manage remote Helm values files.

As of Argo CD 2.12 you can now:

- See information about the revision of both sources for each item in the history.
- Rollback an Application to the revision of both sources from that point in time.

To test this:

1. On the `multi-source-app` `Application`, update the target revision for source 2 (`REPO URL: https://github.com/morey-tech/argo-cd-helm-values-demo`) to `fad7d80f96824e58579d9009b75772f119814c3e` to generate a new entry in the history.
2. Click on `HISTORY AND ROLLBACK` to view the history of the `Application`.
3. Click on the older entry in the history to view the sources info. Then click on the button in the top right of the entry and click `Rollback` to rollback the `Application`.

Timeline:

1. 2023-02-23: Issue #12580 opened.
2. 2023-06-19: Pull request #14124 opened.
3. 2024-06-10: Feature merged.
4. 2024-07-05: 2.12.0 released.
