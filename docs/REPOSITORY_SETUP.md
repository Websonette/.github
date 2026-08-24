# Repository setup checklist

Use this checklist when creating a Websonette package repository. Repository setup is intentionally separate from package implementation.

## General

- [ ] Choose public or private visibility deliberately.
- [ ] Use `main` as the default branch.
- [ ] Add a concise repository description and appropriate topics.
- [ ] Add the package-specific license before publishing a public release.
- [ ] Add `README.md`, `CHANGELOG.md`, and `.gitignore` as appropriate.
- [ ] Add `.github/CODEOWNERS` with the actual maintainers of the package.
- [ ] Enable automatic deletion of merged branches.
- [ ] Prefer squash merging unless the repository needs a different history model.

## CI and dependency management

- [ ] Add a caller workflow based on an organization workflow template.
- [ ] Ensure the package exposes the commands required by the shared CI contract.
- [ ] Verify CI on every supported runtime version.
- [ ] Configure Dependabot for the package managers used by the repository.
- [ ] Keep workflow permissions read-only unless a job explicitly publishes a release.
- [ ] Never expose publishing credentials to pull-request workflows.

## Main branch

- [ ] Require a pull request before merging when more than one maintainer is involved.
- [ ] Require CI status checks before merging.
- [ ] Require conversations to be resolved before merging.
- [ ] Block force pushes and deletion of `main`.
- [ ] Do not require an approving review for a single-maintainer repository unless there is another reviewer.

Organization-wide rulesets are not enforced on the current GitHub Free plan. Configure these rules per repository where the plan and repository visibility support them.

## Public packages

- [ ] Confirm that the repository contains no private code, client information, secrets, or internal infrastructure details.
- [ ] Confirm package metadata and namespace ownership.
- [ ] Publish Composer packages through Packagist and Node packages through the selected npm registry.
- [ ] Create immutable, versioned releases only after CI passes.

## Private packages

- [ ] Grant access through teams or explicit repository roles, not shared credentials.
- [ ] Use a read-only deploy key, fine-grained token, or GitHub App for CI consumers.
- [ ] Document how Composer or npm authenticates without committing credentials.
- [ ] Review access before adding the package to another application.

