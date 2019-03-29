# Kubernetes

## Releasing to non-production

1. Wait for [the Docker build to complete on CircleCI](https://circleci.com/gh/ministryofjustice/<repo_name>) for the feature branch.
1. Approve the pending staging deployment on CircleCI.
    [Watch the how-to video:](https://www.youtube.com/watch?v=9JovuQK-XnA)<br/>
    [![How to approve staging deployments](https://img.youtube.com/vi/9JovuQK-XnA/1.jpg)](https://www.youtube.com/watch?v=9JovuQK-XnA)
1. :rotating_light: Unfortunately, our deployment process does not _yet_ fail the build if the deployment fails.
    To see if the deploy was successful, follow Kubernetes deployments, pods and events for any feedback:
    ```
    kubectl --namespace <name-space-name> get pods,deployments -o wide
    kubectl --namespace <name-space-name> get events
    ```

## Releasing to production

1. Please make sure you tested on a non-production environment before merging.
1. Merge your feature branch pull request to `master`.
1. Wait for [the Docker build to complete on CircleCI](https://circleci.com/gh/ministryofjustice/<repo_name>/tree/master) for the `master` branch.
1. Approve the pending staging deployment on CircleCI (see 'Releasing to non-production above' video for more info).
1. Approve the pending production deployment on CircleCI.
1. :rotating_light: Unfortunately, our deployment process does not _yet_ fail the build if the deployment fails.
    To see if the deploy was successful, follow Kubernetes deployments, pods and events for any feedback:
    ```
    kubectl --namespace <name-space-name> get pods,deployments -o wide
    kubectl --namespace <name-space-name> get events
    ```

## Rolling back

1. Check the rollout history with `kubectl rollout history deployment/<deployment> --namespace=<name-space-name>`
1. Roll back to the previous version with `kubectl rollout undo deployment/<deployment> --namespace=<name-space-name>`

:memo: The `<name-space-name>` is the applications kubernetes namespace. `kubectl get namespaces | grep laa-cla` gets you a list of laa-cla namespaces

:memo: The `<deployment>`> is the kubernetes deployment name. `kubectl -n <namespace> get deployments` gets you the deployment name
