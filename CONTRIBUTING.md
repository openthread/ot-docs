# Contributing to OpenThread Docs

We would love for you to contribute to OpenThread and help make it even better than it is today! As a contributor, here are the guidelines we would like you to follow.

Documentation contributions in this repo may be mirrored on our [openthread.io](https://openthread.io) website.

- [1 Code of Conduct](#code-of-conduct)
- [2 Bugs](#bugs)
- [3 New Documentation](#new-documentation)
- [4 Contributing Content](#contributing-content)
  - [4.1 Initial Setup](#initial-setup)
  - [4.2 Contributor License Agreement (CLA)](#contributor-license-agreement--cla-)
  - [4.3 Submitting a Pull Request](#submitting-a-pull-request)

## Code of Conduct

Help us keep OpenThread open and inclusive. Please read and follow our [Code of Conduct](CODE_OF_CONDUCT.md).

## Bugs

If you find a bug in the documentation, you can help us by [submitting a GitHub Issue](https://github.com/openthread/ot-docs/issues/new). The best bug reports provide a detailed description of the issue and step-by-step instructions for predictably reproducing the issue. Even better, you can [submit a Pull Request](#submitting-a-pull-request) with a fix.

## New Documentation

You can request new documentation by [submitting a GitHub Issue](https://github.com/openthread/ot-docs/issues/new).

If you would like to implement new documentation, please consider the scope of the content:

- _Large_: first [submit a GitHub Issue](https://github.com/openthread/ot-docs/issues/new) and communicate your proposal so that the community can review and provide feedback. Getting early feedback will help ensure your implementation work is accepted by the community. This will also allow us to better coordinate our efforts and minimize duplicated effort.

- _Small_: can be implemented and directly [submitted as a Pull Request](#submitting-a-pull-request).

## Contributing Content

The OpenThread Project follows the "Fork-and-Pull" model for accepting contributions.

### Initial Setup

Setup your GitHub fork and continuous-integration services:

1. Fork the [OpenThread repository](https://github.com/openthread/ot-docs) by clicking "Fork" on the web UI.

Setup your local development environment:

```bash
# Clone your fork
git clone git@github.com:<username>/ot-docs.git

# Configure upstream alias
git remote add upstream git@github.com:openthread/ot-docs.git
```

### Contributor License Agreement (CLA)

Contributions to this project must be accompanied by a Contributor License Agreement. You (or your employer) retain the copyright to your contribution; this simply gives us permission to use and redistribute your contributions as part of the project. Head over to <https://cla.developers.google.com/> to see your current agreements on file or to sign a new one.

You generally only need to submit a CLA once, so if you've already submitted one (even if it was for a different project), you probably don't need to do it again.

### Submitting a Pull Request

#### Branch

For each new feature, create a working branch:

```bash
# Create a working branch for your new feature
git branch --track <branch-name> origin/main

# Checkout the branch
git checkout <branch-name>
```

#### Create Commits

```bash
# Add each modified file you'd like to include in the commit
git add <file1> <file2>

# Create a commit
git commit
```

This will open up a text editor where you can craft your commit message.

#### Upstream Sync and Clean Up

Prior to submitting your pull request, you might want to do a few things to clean up your branch and make it as simple as possible for the original repo's maintainer to test, accept, and merge your work.

If any commits have been made to the upstream main branch, you should rebase your development branch so that merging it will be a simple fast-forward that won't require any conflict resolution work.

```bash
# Fetch upstream main and merge with your repo's main branch
git checkout main
git pull upstream main

# If there were any new commits, rebase your development branch
git checkout <branch-name>
git rebase main
```

Now, it may be desirable to squash some of your smaller commits down into a small number of larger more cohesive commits. You can do this with an interactive rebase:

```bash
# Rebase all commits on your development branch
git checkout
git rebase -i main
```

This will open up a text editor where you can specify which commits to squash.

#### Conventions and Style

OpenThread uses and enforces the [Documentation Style Guide](./STYLE_GUIDE.md) on all documentation.

#### Push and Test

```bash
# Checkout your branch
git checkout <branch-name>

# Push to your GitHub fork:
git push origin <branch-name>
```

#### Submit Pull Request

When ready, go to the page for your fork on GitHub, select your development branch, and click the pull request button. If you need to make any adjustments to your pull request, just push the updates to GitHub. Your pull request will automatically track the changes on your development branch and update.
