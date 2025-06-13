<div align="center">
<h1>Git Flow & Release Templates</h1>

A codebase aimed at fine-tuning GitHub repository releases while utilizing the Git Flow.

</div>

## About

This repository project provides a structured approach to managing GitHub repositories, focusing on streamlined branch workflows, effective label management, and automated release note generation. 

By leveraging tools like `release-drafter` and custom branch protection rules, this repository aims to enhance collaboration and maintain a consistent development process.

Whether you're working on a small project or a large-scale application, this template serves as a foundation for efficient version control and release management.

![gitflow image](./docs/gitflow.drawio.svg)

## Examples of specific development flow

See `Development Flows` section in `CONTRIBUTING.md`.

## Get Ready

Describe how to use `Git Flow & Release Templates` in your own environment after cloning or forking it.

### Create Branches

- Create a `develop` branch from the `main` branch.

### Settings

#### Default Branch

- `Settings > General > Default branch`: Set `develop` as the default branch.

#### Branch Protection Rules

- TODO: 

### Create Labels (for PR)

The following labels should be created by default:  

- bug
- documentation
- duplicate
- enhancement
- good first issue
- help wanted
- invalid
- question
- wontfix

See: [https://docs.github.com/en/issues/using-labels-and-milestones-to-track-work/managing-labels#about-default-labels](https://docs.github.com/en/issues/using-labels-and-milestones-to-track-work/managing-labels#about-default-labels)


Here, we add the labels (fix, refactor, chore) used in this project.

```sh
gh label create fix --description "Bug fix" --color d73a4a
gh label create refactor --description "Code change that neither fixes a bug nor adds a feature" --color 5319e7
gh label create chore --description "Other changes that don't modify src or test files" --color cfd3d7
```

> [!TIP]
> If you want to manage more labels, it might be helpful to use [Financial-Times/github-label-sync](https://github.com/Financial-Times/github-label-sync).  
> A similar library is [micnncim/action-label-syncer](https://github.com/micnncim/action-label-syncer), but it seems to no longer be actively maintained.


### Set Github PAT(Classic)

Set up a GitHub Personal Access Token (Classic) for `.github/workflows/create-release-pr.yml`.

The required permissions are as follows:

- [x] repo
- [x] workflow
- [ ] write:packages
  - [x] read:packages

And name the PAT as `RELEASE_WORKFLOW_PAT`.

### Done!

Now you're ready to use this repository!


## Details

### Release Drafter

This project uses [release-drafter](https://github.com/release-drafter/release-drafter) to create release notes. Draft creation is triggered by the `.github/workflows/create-release-and-tag.yml` workflow when a merge from the release branch to the main branch occurs.

For more details, refer to `CONTRIBUTING.md`.