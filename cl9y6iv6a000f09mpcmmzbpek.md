# Caching docker layers in github actions and retriggering workflow

Following my previous post on [automating infrastructure provisioning, configuration and application deployment](https://www.yaphc.com/automating-infrastructure-provisioning-configuration-and-application-deployment), there are 2 areas that I wanted to improve.

## Cache docker layers in deploy workflow 
First, docker layers in the build step of the [deploy workflow](https://github.com/hanchiang/url-shortener-backend/blob/master/.github/workflows/deploy.yml) should be cached.

The [build-push-action](https://github.com/docker/build-push-action) github action can build, push, and cache the docker image.

%%[urlshortener-github-actions-cache-docker-layers]

With this simple modification, the deployment workflow is shortened by 3 minutes. 

This may not be significant, but it is useful for me as I have a script to start and stop the application, which is triggered on schedule via github actions in the[infra repository](https://github.com/hanchiang/url-shortener-infra/tree/master/.github/workflows).

I can also run the script manually when I am debugging an issue, whether it is on infrastructure configuration or application deployment.

## Dispatch workflow, don't re-run workflow job
Next, [dispatch a workflow](https://docs.github.com/en/rest/actions/workflows#create-a-workflow-dispatch-event) instead of [re-running a workflow job](https://docs.github.com/en/actions/managing-workflow-runs/re-running-workflows-and-jobs).

This is because the maximum log retention is 30 days, so re-running a job that is older than 30 days will fail.
> You can re-run a workflow run, all failed jobs in a workflow run, or specific jobs in a workflow run up to 30 days after its initial run.

```
deploy_workflow=$(curl  -H "Accept: application/vnd.github+json" -H "Authorization: token $GITHUB_TOKEN" https://api.github.com/repos/hanchiang/url-shortener-backend/actions/workflows |  jq '.workflows[] | select(.state == "active" and select(.name | ascii_downcase | contains("build and deploy")))')

workflow_id=$(echo $deploy_workflow | jq -r .id)
curl -X POST  -H "Accept: application/vnd.github+json" -H "Authorization: token $GITHUB_TOKEN" https://api.github.com/repos/hanchiang/url-shortener-backend/actions/workflows/$workflow_id/dispatches \
 -d '{"ref":"master"}'
```

Before this modification, I have to manually dispatch a workflow in the github actions UI every 30 days. With this, the entire infrastructure configuration and application deployment pipeline is now fully automated 😄

