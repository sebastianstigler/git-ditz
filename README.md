git-ditz
========

Git-integration for ditz.

* requires git >= 1.7.7

Allows to run

    git ditz <any-ditz-command>

in your git repository. It can automatically commit your new/modified
issues and can handle the case that the issues live in a different
branch than your project. It stores its configuration in git-config variables.

The following git-variables can be set:

* `ditz.branch` - name of the branch where the ditz-issues are located
* `ditz.executable` - path to the ditz-executable to use
* `ditz.post-hook` - string that is eval'ed after running ditz with all commands
* `ditz.post-<command>-hook` - string that is evaled after running ditz with command `<command>`
* `ditz.auto-commit` - if set, automatically commit changes made to the ditz files by ditz
* `ditz.auto-commit-message` - if set, use this message for auto commits
* `ditz.post-commit-hook` - string that is eval'ed after automatically committing (needs to be enabled)
* `ditz.central-repo` - path to the repo which contains the ditz.branch

## Example ##

### Example 1

Ditz notes live in a different branch called "issues"

    git config ditz.branch issues
    git config ditz.auto-commit t

### Example 2

Ditz notes live in a different branch called "gh-pages".
After each update of the ditz database, update the html and push to
github (such that the github-page reflects the current state of you
ditz database).

    git config ditz.branch gh-pages
    git config ditz.auto-commit t
    git config ditz.post-commit-hook "ditz html; git add html/*; git commit -a -m \"updated html\"; git push"

Now, after every commit (which happens after every change to ditz'
database), the html is refreshed and pushed.

### Example 3

You want to track the issues of some (sub-)repositories in a central repository.
You want to invoke `git ditz ...` within the (sub-)repositories but want to work
with the issue database in the central repository.

Within the central repository (just like Example 2):

    git config ditz.branch gh-pages
    git config ditz.auto-commit t
    git config ditz.post-commit-hook "ditz html; git add html/*; git commit -a -m \"updated html\"; git push"

Assume the central repository is located at `/path/to/central/repository`.
Within the (sub-)repositories but not the central repository:

    git config ditz.central-repo /path/to/central/repo

Now you can invoke `git ditz ...` either in the (sub-)repositories or in the
central repository and you always work with the Ditz database in the central
repository.
