# Gitflow
> The git branching model discussed here is a variant of [`Vincent Driessen`](http://nvie.com/posts/a-successful-git-branching-model/) work, a good introduction can be found [here](https://datasift.github.io/gitflow/IntroducingGitFlow.html)

## Purpose
The purpose of introducing gitflow is to reduce complexity in project deployment process and also to allow Continous Integration and Deployment to work correctly

## Important Branches
There are two important permanent branches in current implementation
- `develop` branch: contained the most updated development work of `Managing Project`. no commit should be committed to this branch directly
- `production` branch: it is used for production deployment process. same as `develop` branch no commit should be done here directly
- `release` branch: to simplify the adapted gitflow, this branch will be ignored in current implementation.

## Start a new Project
This section described the steps required to start a new project.

* First sync the `develop` branch
```
git pull origin develop
```
* Create a new branch (hereinafter `feature` branch) from `develop` branch. `feature` branch named by issue number
```
git checkout -b 089-clean-up-code
```
* Start hacking on the new branch...
* When project development done, rebase the with `develop` branch again
```
git rebase develop
```
* push the `feature` branch to staging server for QA test
* When QA done, create a `merge request` on gitlab for peer code review
* When code review done, merge it back to `develop` by clicking to the `squash merge` button on gitlab

## Deployment to Production
This section descibed the steps to deploy `production` branch to production server

* when the `develop` branch is ready for deployment, merge the branch to `production`
```
git merge develop
```
* create a tag on `production` branch
```
git tag -a v0.0.1 -m 'cleaned up code'
git push origin --tags
```
* ssh to live server, and pull the tag to working directory
```
git fetch --all --tags
git pull tags/v0.0.1 -b v0.0.1
```
* restart pm2
```
pm2 reload managing-projects
```

## Handle hotfix
This section descibed how to handle hotfix for ifxing production issues

* create a branch directly from production branch
```
git checkout -b hotfix-fix-mlp-issue
```
* start hacking on the new branch
* when development is done, do QA on the branch, in staging or even in production
* when QA is done, merge back to `production` branch, follow the deployment [steps](#Deployment-to-Production)
* merge the hotfix back to `develop` branch
```
git merge hotfix-fix-mlp-issue
```
