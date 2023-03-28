# dalamud-github-actions
Just a collection of CI/CD for Dalamud plugins with Github actions

## Default Action Triggers

Refer to the YAML syntax from Github's official guide @ https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#about-yaml-syntax-for-workflows

|Branch Name | Event | Builds| Deploy |
|---|---|---|---|
|main | any new commits to the branch | YES | Only if the commit is tagged |
|main | new pull request to merge into `main` | YES | NO |
| other branches | any new commits to the branch | NO | NO |
| other branches | new pull request to merge into target branch | NO | NO |


## How to use

Prerequisite:
* ensure `dotnet store` and `dotnet build` can start from root of the repo
* this uses dotnet 7.x, ensure repo is dotnet 7.x compatible (or update version from the yaml file)
* your FFXIV plugin only uses Dalamud as a third party dependencies and nothing else

1. Copy what you need from [build_and_deploy.yaml](./.github/workflows/build_and_deploy.yaml)
2. Create the same folders ./github/workflows from your own root repository if it doesn't exist
3. create a new `.yaml` file, paste the content and adjust anything you need, by default it uses the repo project name as the zip file names
4. commit the new file and push to github
5. go to project setting and ensure Github Actions is enabled
6. Now this CI/CD should be in effect!

## Deploy

To deploy, go to the terminal and do

```sh
git checkout main
git commit -m "update"
git tag -a v1.0 -m "v1.0 release"
git push --follow-tags
```

Upon pushing a tag on any commits to `main`, a Github action will be triggered to follow the build and deploy step.

## Closing thought

We created this to document our attempt to automate the build and deploy from another open source plugin. We do not have the dotnet environment setup locally so using Github Actions made the most sense back then.
This Github Action is missing an end-to-end testing CI... we might add one later just to prove this would works as intended.
