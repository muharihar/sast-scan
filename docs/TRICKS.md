# Tips and tricks

This page captures advanced customisation and tweaks supported by sast-scan.

## Workspace path prefix

sast-scan tool is typically invoked using the docker container image with volume mounts. Due to this behaviour, the source path the tools would see would be different to the source path in the developer laptop or in the CI environment.

To override the prefix, simply pass the environment variable `WORKSPACE` with the path that should get prefixed in the reports.

```bash
export WORKSPACE="/home/appthreat/src"

# To specify url
export WORKSPACE="https://github.com/appthreat/cdxgen/blob/master"
```

If your organization use `Azure Repos` for hosting git repositories then the above approach would not work because of the way url gets constructed. You can construct the url for Azure Repos as follows:

```bash
export WORKSPACE="$(Build.Repository.Uri)?_a=contents&version=GB$(Build.SourceBranchName)&path="
```

However, note that because of the way `Build.SourceBranchName` is [computed](https://docs.microsoft.com/en-us/azure/devops/pipelines/build/variables?view=azure-devops&tabs=yaml) this variable may not work if the branch contains slashes in them such as `feature/foo/bar`. In such cases, the branch name has to be derived based on the variable `Build.SourceBranch` by removing the `/refs/heads` or `/refs/pull/` prefixes.

Let us know if you find a better way to support direct linking for Azure Repos.