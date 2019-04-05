# Template deploy

## Releasing to non-production

1. Wait for [the Docker build to complete on CircleCI (`https://circleci.com/gh/ministryofjustice/<repo_name>`) for the feature branch.
1. From the output of the `Tag and push Docker images` job, note the tag pushed to the DSD docker registry, e.g.
    ```
    Pushing tag for rev [9a77ce2f0e8a] on {https://registry.service.dsd.io/v1/repositories/<repo_name>/tags/some-feature-branch.latest}
    ```
1. Use Jenkins to deploy your branch `https://ci.service.dsd.io/job/DEPLOY-<repo_name>/build?delay=0sec`.
    * `APP_BUILD_TAG` is the tag name you noted in the previous step: the branch name, a dot separator, and `latest` e.g.`some-feature-branch.latest`
    * `environment` is the target environment, select depending on your needs, e.g. `demo` or `staging`
    * `deploy_repo_branch` is the deploy repo's (`https://github.com/ministryofjustice/<repo_name>-deploy`) default branch name, usually `master`.

## Releasing to production

>#### :warning: Release to CLA_BACKEND / CLA_FRONTEND production outside of business hours
> __Business hours__: 09:00 to 20:00  
>__Why?__ Any downtime on the frontend and backend between 09:00 and 20:00 can have serious consequences, leading to shut down of the court legal advice centres, possible press reports and maybe MP questions.  
>__Is there downtime when a release occurs?__ Usually it's just a few seconds. However changes that involve Elastic IPs can take a bit longer.

1. Please make sure you tested on a non-production environment before merging.
1. Merge your feature branch pull request to `master`.
1. Wait for the Docker build to complete on CircleCI (`https://circleci.com/gh/ministryofjustice/<repo_name>/tree/master`) for the `master` branch.
1. Copy the `master.<sha>` reference from the `build` job's "Push Docker image" step. Eg:
    ```
    Pushing tag for rev [d96e0157bdac] on {https://registry.service.dsd.io/v1/repositories/<repo_name>/tags/master.b24490d}
    ```
1. Deploy `master.<sha>` to **prod**uction `https://ci.service.dsd.io/job/DEPLOY-<repo_name>/build?delay=0sec`.

:tada: :shipit: