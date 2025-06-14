`HEAD` - pointer to active, current commit.

`git commit -m "message"` - do a commit
`git commit -am` - commit with prev message
`git commit --amend` - update last commit with changes
`git branch <name>` - create new branch `<name>`
`git branch -f main HEAD~3` - move branch main 3 commits before HEAD, but not checkout. (Also, commit hash usage is possible)
`git branch -u origin/<sourceBranch> <localBranch>` - make localBranch track source, do later pulls and pushes to it.

`git checkout <commit|branch>` - shift HEAD to a specific branch or commit. Usually HEAD points to a branch. If we checkout it to a specific commit HEAD becomes detached.
`git checkout -b <newBranch>` - create new branch and checkout
`git checkout -b <newBranch> origin/<anotherBranch>` - checkout to new branch and make it track distinct branch, do later pulls and pushes to it.
`git checkout main^` - move HEAD to last main commit parent. `main^^` - to grandparent, etc. Also, you can use HEAD^. For merge commits you can specify HEAD^2 to move to the second parent.
`git checkout main~5` - move HEAD 5 commits backward from main last commit.
`git checkout HEAD~3^2~` - move HEAD 3 commits back, then to 2 parent and 1 commit back again.
`git reset HEAD~2` - remove 2 last commits in local repo
`git revert cx` - do commit with reflected changes of commit cx
`git rebase -i HEAD~4 | git rebase -i <tagretBranch>` - interactive rebase; apply commits to HEAD, but first allow to squash them, reorder or skip - and apply after that.
`git tag t1 c1` - add tag t1 to commit c1 (you can checkout to tag after that)
`git describe <smth||HEAD>` - how far `<smth>` from the closest tag (`<tag>_<numCommits>_g<hash>`)
`git fetch` - fetch data from remote repo to your local representation of remote repo (origin).
`git fetch origin foo` - fetch remote foo to origin/foo independently of where HEAD is now
`git fetch origin foo~2:bar` - fetch remote commits from (foo-2 commits) to local bar (not origin)
`git fetch origin:bar` - create bar branch locally (WTF???)
`git pull` - fetch + merge ff if current branch is behind the remote (just shifts HEAD to last commit) || fetch + merge if it's required to reconcile the divergent branches
`git pull --rebase` - fetch + rebase
`git pull origin foo:bar` - fetch commits from remote foo to local bar and merge with current branch
`git push origin foo` - push foo to origin\remote independently of where HEAD is now
`git push origin foo^:bar` - push (foo branch-1 commit, (commit hash could be here too)) to origin/bar
`git push origin:foo` - delete foo locally and remotely (WTF???)
`git log --reflog --author "Stanislav Tretiakov"` - watch removed commits of mine
`git merge --squash <source-branch>` - squash commit to `<source-branch>`
`git push origin +<branch-name> –force` - override remote commits with local
`git reset --hard origin/<branch-name>` - override local commits with remote
`git update-index --chmod=+x <file-path>` - update file permissions.

**Git-indexes are saved during environments (different machines)**
### If git commands work very long:
- First of all, try to execute following command in any repository: `GIT_CURL_VERBOSE=1 GIT_TRACE=1 git fetch`
- If it hangs you need to update file `c:\windows\system32\drivers\etc\hosts`
- Add actual IP address vs server hostname: `12.23.34.45 git-server.com`

[source](https://www.howtogeek.com/howto/27350/beginner-geek-how-to-edit-your-hosts-file/)

```.gitconfig
[remote "origin"]
    url = https://<login>:<password>@<git>.git
[credential "https://server.com"]
    username = <username>
    password = <password>
```
