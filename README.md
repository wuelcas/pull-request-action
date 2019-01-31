# Automated Branch Pull Requests

This action will open a pull request to master branch (or otherwise specified)
whenever a branch with some prefix is pushed to. The idea is that you can
set up some workflow that pushes content to branches of the repostory,
and you would then want this push reviewed for merge to master.

Here is an example of what to put in your `.github/main.workflow` file to
trigger the action.

```
workflow "Create Pull Request" {
  on = "push"
  resolves = "Create New Pull Request"
}

action "Create New Pull Request" {
  uses = "vsoch/pull-request-action@master"
  env = {
    BRANCH_PREFIX = "update/"
    PULL_REQUEST_BRANCH = "master"
  }
}
```

Environment variables include:

  - **BRANCH_PREFIX**: the prefix to filter to. If the branch doesn't start with the prefix, it will be ignored
  - **PULL_REQUEST_BRANCH**: the branch to issue the pull request to. Defaults to master.

## Example use Case: Update Registry

As an example, I created this action to be intended for an [organizational static registry](https://www.github.com/singularityhub/registry-org) for container builds. Specifically, you
have modular repositories building container recipes, and then opening pull requests to the 
registry to update it. 

 - the container collection pull request should be generated from a separate GitHub repository, including the folder structure (manifests, tags, collection README) that are expected.
 - pushing a branch that starts with update/<namespace> should open a pull request, if it doens't exist. If the branch is already open for PR, it updates it.