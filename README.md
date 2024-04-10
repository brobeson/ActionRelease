# Action Release

[![Workflow Quality](https://github.com/brobeson/ActionRelease/actions/workflows/workflow_quality.yaml/badge.svg)](https://github.com/brobeson/ActionRelease/actions/workflows/workflow_quality.yaml)
![GitHub Release](https://img.shields.io/github/v/release/brobeson/ActionRelease?logo=github)

This is a reusable workflow to tag and release a new version of a GitHub action or workflow.
The workflow takes a [semantic version](https://semver.org) as input.
It validates the version, tags the repository on the `main` branch, and creates a GitHub Release.

The input version must be a 3-component semantic version: `A.B.C`.
(The version may have a `v` prefix; the workflow accounts for that.)
It then creates the tag `vA.B.C`.
It also force creates the tags `vA.B` and `vA` at the same commit.
This lets users of your workflow automatically get updates that they want.

## Getting Started

To use this workflow, just set it in the `uses` key of a job in your project's workflow file.
Here is an example workflow snippet that calls this workflow with a hard code `1.0.0` version:

```yaml
jobs:
  release:
    name: New Release
    uses: brobeson/ActionRelease/.github/workflows/release.yaml@v1
    with:
      version: "1.0.0"
```

### Manually Running Your Workflow

One method to pass a new version to this workflow, is for your calling workflow to use `on.workflow_dispatch` and require a version input.

```yaml
on:
  workflow_dispatch:
    inputs:
      version:
        description: The new version.
        required: true
        type: string
jobs:
  release:
    name: New Release
    uses: brobeson/ActionRelease/.github/workflows/release.yaml@v1
    with:
      version: ${{inputs.version}}
```

> [!WARNING]  
> The action release workflow will fail if you try to automatically run it when you create a tag in your repository.
> I'll address this in a future version of the action release workflow.

## Inputs

<!-- prettier-ignore -->
| Key       | Required |  Type  | Description                              |
| :-------- | :------: | :----: | :--------------------------------------- |
| `version` |   yes    | string | The [semantic version](https://semver.org) of the new release. |

## Steps

The workflow runs the following steps.

1. **Verify the Version**  
   The version must be a 3-component [semantic version](https://semver.org).
   This step verifies the input.
   A `v` prefix, such as `v1.0.0`, is allowed; the workflow strips it out.
1. **Clone the Repository**  
   This step just clones the repository to the runner.
1. **Tag the Repository**  
   This step creates the local tags `vA.B.C`, `vA.B`, and `vA` on the `main` branch.
   The tags `vA.B` and `vA` are created with `--force`; if they already exist, they move to the new commit.
1. **Push the Tags**  
   This step force pushes the new tags to the origin remote.
1. **Create a Release**  
   This step creates a new GitHub Release at the new `vA.B.C` tag.
   The release title is `A.B.C`.
   For now, the release notes are empty.

## Issue Tracking

[![GitHub Issues or Pull Requests by label](https://img.shields.io/github/issues/brobeson/ActionRelease/bug?logo=github&label=Bugs)](https://github.com/brobeson/ActionRelease/issues?q=is%3Aissue+is%3Aopen+label%3Abug)
[![GitHub Issues or Pull Requests by label](https://img.shields.io/github/issues/brobeson/ActionRelease/enhancement?logo=github&label=Enhancements)](https://github.com/brobeson/ActionRelease/issues?q=is%3Aissue+is%3Aopen+label%3Aenhancement)
[![GitHub Issues or Pull Requests by label](https://img.shields.io/github/issues/brobeson/ActionRelease/new%20step?logo=github&label=New%20Steps)](https://github.com/brobeson/ActionRelease/issues?q=is%3Aopen+is%3Aissue+label%3A%22new+step%22)
[![GitHub milestone details](https://img.shields.io/github/milestones/progress/brobeson/ActionRelease/1?logo=github)](https://github.com/brobeson/ActionRelease/milestone/1)

[Report a bug](https://github.com/brobeson/ActionRelease/issues/new?assignees=brobeson&labels=bug&projects=&template=bug.yaml) |
[Request a new step](https://github.com/brobeson/ActionRelease/issues/new?assignees=brobeson&labels=new+step&projects=&template=new_step.yaml) |
[Update an existing step](https://github.com/brobeson/ActionRelease/issues/new?assignees=brobeson&labels=enhancement&projects=&template=enhancement.yaml)
