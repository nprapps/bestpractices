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

1. `git pull --rebase` - Get any commits that may have occurred since the last time you synchronized with the repo. You can set `pull` to always rebase, if you have trouble remembering the option.
1. If you have conflicts, use `git stash` to store your local changes, pull again, then `git stash pop` to unshelve your changes back into the folder.
1. `git status` to check that there aren't any spurious deletions or oddities.
1. Finally, `git commit` to add changes and `git push` to send them to the remote repo.

