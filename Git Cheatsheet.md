# Git Cheatsheet

*Cleaning stale branches on local machine* - `git remote prune <remote>` e.g. `git remote prune origin`

*Delete a local branch*
* `git branch -d <branch_name>`
* `git branch -D <branch_name>`, -D is an alias for `--delete --force` which deletes the branch "irrespective of its merged status".

*Delete a remote branch* - `git push -d <remote_name> <branch_name>`

*Set Upstream (tracking) branch* - `git branch --set-upstream-to <origin/remote-branch>`

*Unstage commits* - `git reset --soft HEAD~<number of commits>`

*Push new branch and set upstream* - `git push --set-upstream origin`