# Contributing to TAO

> Information regarding the process for contributing to TAO.

Contributions to the TAO codebase are made using the fork & pull model. This contribution model has contributors maintaining their own copy of the forked codebase. The forked repository can easily be synced with the main copyi by submitting a “pull request” to merge the set of changes. For more information on pull requests, please refer to [GitHub Help](https://help.github.com/articles/about-pull-requests/).

The TAO development team will review all issues and contributions submitted by the community of developers utilizing a first in, first out order. During the review, we might require clarifications from the contributor and if there is no response from the contributor within two weeks, the pull request will be closed.

## Contribution process

If you are unfamiliar with Git, please see our [Git Basics](git-basics.md) page for information on installing Git, getting a GitHub account, and some basics commands.

Once you are configured, you will utilize the following process to contribute:

1. Check the open and closed issues for similar proposals of intended contribution before starting work on a new contribution.
2. Fork the repository of the TAO extension you wish to contribute.
3. Create a branch that follows the [GitFlow](https://datasift.github.io/gitflow/IntroducingGitFlow.html) branching model.
4. Create and test your work.
5. Once development has been done, create a pull request that targets the development branch of the extension you are contributing to.
6. If your code depends on changes in another extension, create a draft pull request until all required pull requests are created.
7. Once your contribution is received the TAO development team will review the contribution and collaborate with you as needed.

### Templates

The following templates are designed to help ensure all relavant information in included in Issues that created as well a checklist to include in your Pull Request.

Issue Template

```
### Subject of the issue
Describe your issue here.

### Your environment
* Which browser and version are you using?
* Which PHP version are you using?
* Which Database engine and version are you using?
* Which Web server are you using?
* Which extensions are installed, and what version are they?

### Steps to reproduce
Tell us how to reproduce this issue.

### Expected behaviour
Tell us what should happen

### Actual behaviour
Tell us what happens instead

```

Pull Request Template

```
_Before you submit a pull request, please make sure you have to following:_

- [ ] The title of this pull request offers a good description of what is changed (as it is used in release notes).
- [ ] Your branch follows the [GitFlow](https://datasift.github.io/gitflow/IntroducingGitFlow.html) branching model.
- [ ] The code follows the [best practices (to be defined)](#).
- [ ] The functionality has been manually tested (if applicable).
- [ ] The update script has been run, and causes no issues.
- [ ] The functionality has been tested after a clean install.
- [ ] A new unit test has been created, or the existing test has been updated.
- [ ] All new and existing tests passed.
- [ ] The module version has been bumped in both the manifest.php, and Updater.php files.

---
**Depends on**
- [ ] List other pull requests that depend on this pull request
- [ ] Also list pull requests that require this pull request
---

Describe the changes you made in your pull request here

**Testing the changes**

Please provide a description of how to test the changes made in this pull request.
```
