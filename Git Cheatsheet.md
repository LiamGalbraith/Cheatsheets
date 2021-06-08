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
      * Create a `git uncommit` alias: `git config --global alias.uncommit 'reset HEAD^'`



*Push new branch and set upstream* - `git push --set-upstream origin`

*Merge a branch into the current branch* - `git merge <branch_name>`

## Scenarios

### Safely reversing a rebase using 'reflog', 'tag', and 'reset'
Hopefully, this shouldn't happen, but if you need to reverse a rebase, there is a simple solution:
1. Create a backup, just incase: `git tag BACKUP`
2. Find the head commit of the branch immediately before the rebase: `git reflog`, or `git log -g`
3. Reset the branch. Suppose the old commit was `HEAD@{3}` in the ref log: `git reset --hard HEAD@{3}` (For Windows, this needs to be `git reset --hard "HEAD@{3}"`
4. If there's an issue, reset to the backup: `git reset --hard BACKUP`
