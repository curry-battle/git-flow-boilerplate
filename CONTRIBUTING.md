## Development Flows

This project adopts a development flow based on Git Flow.

### Basic Development Flow

For regular feature development and improvements, follow these steps:

1. **Create Branch**
   - Create a new `feature/*` branch from the `develop` branch

2. **Development & Commit**
   - Implement features and commit following the commit conventions

3. **Create Pull Request**
   - Create a pull request from `feature/*` → `develop`
   - Apply appropriate labels (`enhancement`, `fix`, `documentation`, etc.)
   - Title format: `Feature: Add user authentication system`

4. **Review & Merge**
   - After code review completion, merge using **Squash and merge**

5. **Delete Branch**
   - Delete the feature branch after merging

### Release Flow

When releasing a new version, follow these steps:

1. **Create Release PR**
   - Manually execute the `create-release-pr.yml` workflow
   - Select version bump type (major/minor/patch)
   - Automatically creates a `release/v*` branch from `develop` and a PR to `main`

2. **Release Preparation**
   - Review the created release PR
   - Adjust version numbers or release notes if necessary
   - Perform final testing and bug fixes

3. **Minor Fixes**
  - Follow this procedure when there are minor fixes needed.
  - Create a branch from the active `release/v*` branch, similar to the Basic Development Flow.
    - In most cases, this will be a `fix/*` branch.
  - Create a pull request from `fix/*` → `release/v*`
  - After review, merge using **Squash and merge**.

> [!TIP]
> You can also create the fix branch from the `develop` branch instead of the active `release/v*` branch.
> (In this case, the later mentioned `Reflect changes from the release branch into develop` is not necessary.)
> However, if `develop` is ahead of `release/v*` at that time, there is a risk of including commits not intended for release along with the fix. Be careful!

4. **Merge to main**
   - Merge the release PR using **Create a merge commit**
   - Ensure the PR title follows the `Release v*` format
   - After merging, `create-release-and-tag.yml` automatically executes and creates a release draft

5. **Merge back to develop**
   - Create a pull request from `release/*` → `develop`
   - Reflect changes from the release branch into develop
     -  merge using **Create a merge commit**

### Emergency Bug Fix Flow

When urgent fixes are needed in the production environment:

1. **Create Hotfix Branch**
   - Create a `hotfix/*` branch directly from the `main` branch

2. **Fix & Test**
   - Implement emergency fixes and conduct thorough testing

3. **Merge to main**
   - Create a pull request from `hotfix/*` → `main`
   - Apply the `fix` label
   - After review completion, merge using **Create a merge commit**

4. **Merge back to develop**
   - Create a pull request from `hotfix/*` → `develop`
   - Reflect hotfix changes into develop
     - merge using **Create a merge commit**

5. **Delete Branch**
   - Delete the hotfix branch after merging

### Merge Rules

Mentioned in `CONTRIBUTING.md`.

### Other Rules

For detailed development rules, continue reading.

## Branching Rules

Follows Git-Flow.

see: [https://nvie.com/posts/a-successful-git-branching-model/](https://nvie.com/posts/a-successful-git-branching-model/)

### Types of Branches

- `main`: For production use. Only accepts PRs from `release` (and `hotfix` in emergencies).
- `release/*`: Branches for release. You make adjustments for release on this branch. Only accepts PRs from `develop`.
- `develop`: Accepts PRs from `feature`, `release` branches.
- `feature/*`: Branches for developing new features or improvements.
- `hotfix/*`: Branches for urgent fixes.

### Branch Naming Conventions

- Feature Addition: `feature/<issue_number>-<brief_description>` (create from `develop`)
- Hotfix: `hotfix/<issue_number>-<brief_description>` (create from `main`)
- Release: `release/v<x>.<y>.<z>` (create from `develop`)

*Note: `<issue_number>-` can be omitted if the modification is not based on an issue.*

#### Examples

- feature/42-user-authentication
- hotfix/56-security-vulnerability-fix
- release/v1.0.0

## Commit Convention

Before you create a Pull Request, please check whether your commits comply with
the commit conventions used in this repository.

When you create a commit we kindly ask you to follow the convention
`category(scope or module): message` in your commit message while using one of
the following categories:

- `feat / feature`: all changes that introduce completely new code or new
  features
- `fix`: changes that fix a bug (ideally you will additionally reference an
  issue if present)
- `refactor`: any code related change that is not a fix nor a feature
- `docs`: changing existing or creating new documentation (i.e. README, docs for
  usage of a lib or cli usage)
- `build`: all changes regarding the build of the software, changes to
  dependencies or the addition of new dependencies
- `test`: all changes regarding tests (adding new tests or changing existing
  ones)
- `ci`: all changes regarding the configuration of continuous integration (i.e.
  github actions, ci system)
- `chore`: all changes to the repository that do not fit into any of the above
  categories

If your PR contains commits that follow these naming conventions, appropriate
tags will be applied to the PR.

## PR Rules

### Basic Requirements

- All PRs must include a description.
- PR titles must follow this format: `Type: Brief description`
  - Example: `Feature: Add user authentication system`
- Whenever possible, focus each PR on a single feature or fix.
- Keep PR sizes manageable to facilitate code reviews.

### Branch to Branch Rules

Create PRs according to the following rules:

- `feature/*` → `develop`: Regular development flow
- `develop` → `release/*`: Creating a release PR
- `release/*` → `main`: Merging into `main` for release
- `release/*` → `develop`: Merging release fixes back into `develop`
- `hotfix/*` → `main`: Emergency fixes
- `hotfix/*` → `develop`: Merging emergency fixes back into `develop`

### Merge Rules

There are three merge methods available on GitHub:

- Create a merge commit
- Squash and merge
- Rebase and merge

see: [https://docs.github.com/ja/pull-requests/collaborating-with-pull-requests/incorporating-changes-from-a-pull-request/about-pull-request-merges](https://docs.github.com/ja/pull-requests/collaborating-with-pull-requests/incorporating-changes-from-a-pull-request/about-pull-request-merges)

Use __Squash and merge__ in the following cases:

- `develop` (regular development flow)
- `release/*`

Use __Create a merge commit__ in the following cases:

- `main`
- `develop` (when backporting from `release/*`)

> [!CAUTION]
> Performing a Squash and merge into `main` will cause commit hash mismatches, leading to conflicts every time subsequent merges into `main` are attempted.

### Release Drafter and PR Labels

To generate release notes using Release Drafter, ensure that PRs from feature branches to the develop branch include one of the following labels:

- `enhancement`: For feature additions or improvements
- `fix`: For bug fixes
- `bug`: For bug reports
- `documentation`: For changes related to documentation
- `chore`: For other changes (not involving `src` or `test` files)
- `maintenance`: For maintenance tasks
- `refactor`: For refactoring (code changes without functional modifications)

> [!Note]
> Release note categorization is based on these labels, not PR names or branch names.  
> (This behavior can be customized in the settings.)

For detailed configuration, refer to `.github/release-drafter.yml`.
