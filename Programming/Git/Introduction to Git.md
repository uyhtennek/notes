[An Introduction to Git and GitHub by Brian Yu - YouTube](https://www.youtube.com/watch?v=MJUJ4wbFm_A)
[Collaboration and Version Control with Git - CS50 Seminars 2021](https://video.cs50.io/S-gBbnBDUhA)
> [!info]
> Git is a command-line version control system.

___

# Git terms

*repositories*
* = the project
* shorts as *repo*

*commit*
* = each svaed version of the repository

*remotes*
* = repositories host
	* a local repository on our device can be associated with a repository host
* usually they're GitHub, GitLab, etc.

*commit hash*
* a hexadecimal number representing a commit
	* changes in each commit are somehow hashed
* recall [[CS50x 5-4 Hash Table|hash function]]

*working copy*
* the repository version we're working on currently, in contrast to the *version repositories* which consist of all of the commits that we made previously

*staging area*
* the set of tracked changes that will be saved in the next commit
___

# Git commands

`git clone <URL>`
* creates a local copy of an existing remote repository
* can do it without a GitHub account
	* unless the repository is private to some accounts

`git init`
* makes the current directory a Git repository

`git add <files>`
* tracks the `<file>` or files by adding them to the staging area

`git commit -m "<commit message>"`
* saves tracked changes
* `-m` specifies a commit message

`git push`
* pushes local commits to the remote repository

`git revert <commit>`
* undo a commit by creating a new commit that does exactly the opposite of the `<commit>` did
* `<commit>` can be specified as a commit hash
	* usually, the first 5, 6 digits are already enough for Git to uniquely identify a commit
	* or we can use the `HEAD~n` syntax
		* `HEAD` represents the latest commit
		* `HEAD~n` would be the `n` commit before the latest
	* eg. a commit from 5 commits ago is `HEAD~5`

`git status`
* displays state of the repo and staging area

`git log`
* displays log of previous commits in the currently checked-out branch

`git diff`
* displays changes made
* we can also do `git diff <commit> <commit>` to see the changes between the 2 commits
___

# Why SSH, not HTTPS?

both are means for authenticating ourselves, using different protocols
to push to, pull from, download, and so on, to work with a repository

for security reason, GitHub has decided to switch entirely to SSH
* supposed to be more secure than using HTTPS

also not just GitHub, we're also pushing towards SSH these days as well
___

# Collaboration

`git push`, again
* pushes local commits to the remote repository

`git pull`
* downloads changes/commits that have been pushed to the remote
* and merges them into our own local repository
* technically `git pull` is a combination of 2 other commands
	* `git fetch` for the first part
	* `git merge` for the second
___

# Branches
think of it as pointers to commits
* each commit points to the previous commit
* then a branch is a thing that points to one of the commits, any commit
	* when ever we created a new commit, the branch that we currently on would point to the new commit instead
* creating a branch doesn't create a commit
	* the newly created branch will point to the commit that we currently at

a default branch would usually be called something like `main`
* but we can also create other branches with other names
* which is a good habit because we don't want to mess up the main branch
	* eg. when developing a new feature, when fixing a bug, etc.

don't push potentially buggy commits onto the main branch, which others are working on
* instead create new branch and commit to that branch and push to that branch
* main branch remains unaffected while 

once we're done with our new feature or bugs on the newly created branch
then we can decide to merge it back to the main branch

This is actually a very common workflow.
___

# Branch commands

`git branch <branch-name>`
* creates a branch
* doesn't switch to the new branch though

`git checkout -b <branch-name>`
* creates a branch
* and then switch to that branch

`git checkout <branch-name>`
* switch to the branch `<branch-name>`

`git push -u origin <branch-name>`
* pushes a branch to the remote repo
	* tells GitHub that a new branch is coming in
* `-u` is same as typing `--set-upstream`
___

# Pull Requests
which is **not** a git feature, they are specific to GitHub
* GitLab, BitBucket, might also have analogous features but possibly with different names
* eg. GitLab - merge request

it's a request to merge in the changes that we've pushed to a branch, back into the main branch
* usually the main branch, not necessarily must be the main branch

A common workflow when working in a group is to
1. Always commit to a feature branch, and never directly to the main branch
2. push the branch to the remote
3. open a pull request
4. have someone else on the team review the pull request
	* they'll either approve it or request changes
5. if approved, merge it into the main branch
___

# Merge Conflicts

if two people both modify the same lines in the same file but in different ways
git panicks and don't know how to merge them
* it causes a merge conflict

```c
int main() {
	void *ptrs[10];
	for (int i = 0; i < 10; ++i)
<<<<<<< HEAD
		ptrs[i] = malloc(i | 1);
=======
		ptrs[i] = malloc(i + 1);
>>>>>>> origin/main
	for (int i = 0; i < 5; ++i)
		free(ptrs[i]);
}
```

to resolve a merge conflict, we ultimately just need to remove the `<<<<<<<`, `=======`, `>>>>>>>` lines
* but surely we need to place back the correct lines of code to make it meaningful
* maybe talk to the person who make the changes to confirm what the code should look like
* Editors like VS Code have convenient graphical UI elements to make resolving conflicts easier

once conflicts are resolved, just commit the resolved changes using `git commit` as usual
___

# extra

> [!tips]
> `submit50` uses git too!

also look up to
* `git reset`
* `git rebase -i`
___
