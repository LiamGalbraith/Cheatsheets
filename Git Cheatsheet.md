# Git Cheatsheet

*Cleaning stale branches on local machine* - `git remote prune <remote>` e.g. `git remote prune origin`

*Delete a local branch*
* `git branch -d <branch_name>`
* `git branch -D <branch_name>`, -D is an alias for `--delete --force` which deletes the branch "irrespective of its merged status".

*Delete a remote branch* - `git push -d <remote_name> <branch_name>`

*Set Upstream (tracking) branch* - `git branch --set-upstream-to <origin/remote-branch>`

*Unstage commits* - `git reset --[soft|hard|mixed] HEAD~<number of commits>`
  
  * `--soft` Discards the commits, and leaves in the changes to stage/index
  * `--mixed` Discards the commits and adds, but leaves the changes in the working directory.
  * `--hard` Discard the commit, add and any changes to the code.
  * If the commit you want to get rid of was the last commit, and you have not done any additional work you can simply use - `git reset HEAD^`



*Push new branch and set upstream* - `git push --set-upstream origin`

*Merge a branch into the current branch* - `git merge <branch_name>`
