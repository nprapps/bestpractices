# Best Practices for Git

Git (through GitHub) is our primary means of collaboration, so it's important that we be able to do so without losing data or time. Unfortunately, Git is also incredibly complex, so this can be difficult to do. The following guidelines attempt to create a workflow that will minimize time spent debugging repositories.

## Reading materials

* [Pro Git](https://git-scm.com/book/en/v2) is available for free and serves as a good explanation of the concepts involved.
* Although we do not use branches heavily, [Git Flow](https://guides.github.com/introduction/flow/) is a good intro to their workflow.

## The basics

A Git repo is a chain of historical snapshots, each of which depends on the previous snapshot. Because there's a strong, cryptographically-signed relationship between entries in the repo history, it's very difficult (if not impossible) to edit history in Git. Our priorities, therefore, are:

* Never check in any secret values or credentials. When possible, don't even keep them in the same folder as the rest of the code, so that you can't commit them accidentally.
* Try to keep history as a single stream of entries, and minimize the number of merges.

When working in a repo, you should commit code fairly regularly. To keep from creating unnecessary merge commits, adding code should involve the following steps:

1. `git pull --rebase` - Get any commits that may have occurred since the last time you synchronized with the repo. This will also work if you've already committed.
1. If you have conflicts, use `git stash` to store your local changes, pull again, then `git stash pop` to unshelve your changes back into the folder.
1. `git status` to check that there aren't any spurious deletions or oddities.
1. Finally, `git add` and `git commit` to save your changes and `git push` to send them to the remote repo.

We strongly recommend using a GUI client for GitHub to simplify this process. Using the [GitHub Desktop](https://desktop.github.com) app, for example, you'll always have a diff view of your current changes before committing, which means it'll be obvious if you are accidentally about to erase data. The client takes care of adding staged changes for you, which simplifies commits. It's also simpler to right-click and revert individual files, or create a complete revert commit to undo a bad merge.

## Do's and Don'ts

### Do...

* ...always `git status` before committing, to make sure that your staged changes look the way you'd expect, and don't touch other files unintentionally.
* ...set `git config --global pull.rebase true` so that your pulls automatically rebase instead of merging, without needing the command line flag.

### Don't...

* ...use `git add .` or `git commit -a` to add all changed files to the index for committing. It's easy to sweep up changes that you didn't mean to make if your branch is out of sync with the main repo.
* ...merge without looking at the commit message and ensuring that all the files and commits listed are ones that you actually want to change. If there's more than 30 of either, you probably have a problem!
* ...commit credentials, or keep any credentials or secret tokens inside of a repository.