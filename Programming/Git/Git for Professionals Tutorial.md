Tutorial Video: https://youtu.be/Uszj_k0DGsg
___
## #1 The Perfert Commit
* Add the __right__ changes!
* __Good__ commit message!

__DO__:
1. Separates commits by different topics. The larger a commit get, the harder to understand.

* make use of git staging
![[git-staging.png]]
* use ``git add -p [file]`` to stage only some parts (hunks) of a script
![[git-add-p.png]]
after doing this and run ``git status``, can see the same script in both the staged and non-staged area. because part of it is staged and the other part isn't.

2. Commit Message
![[git-commit-message.png]]
commit message contains 2 parts: __subject__ and __body__.

The first line is the subject. Write a brief summary of the changes there. After that, insert a empty line, followed with the body, which we place a more detailed explaination of the changes there. But we still should keep it short for readability.
___
## #2 Branching Strategies
>Agree on a Branching Workflow in Your Team
1. Better to have a written best practice so that everyone understand how to avoid mistakes and collisions.
2. Also can help new team members onboard. ("this is how we work here")
---
Strategy 1 - __Mainline Development__ ("Always Be Integrating")
>Always integrate your own work with the work of the team.

A simplified case:
![[git-mainline-development.png]]
All members integrate their work with only 1 main branch.
* There are only few branches to keep track of
* High risk to have large commits because it affects production code
* Must have high-quality testing environment setup. Testing and QA standards must be top notch. __Don't use this model if don't have this__
---
Strategy 2 - __State, Release, and Feature Branches__
![[git-structured-development.png]]
There are multiple different types of branches.
* New feature and experiment are kept in their own branches
* Easy to plan and manage because each branch is representative:
	* production or main branch is used for releases; 
	* develop branch is used by development team only;
	* feature branch is used for a specific feature
---
>In reality most teams are working somewhere between _Mainline Development_ and _Structured Branching Strategy_.

The key is to control __Long-Running__ and __Short-Lived__ Branches well.

_Long-Running_ branch:
* Every project at least has 1 branch exist through the complete lifetime of the project (usualy is main or master)
* Often they mirror "stages" in development life cycle (didn't understand...)
* Common convention is never directly commit to these branches. Only through merge or rebase.
	* We don't want untested and unreviewed code to the production environment
	* Maybe we want to release new code in batches or scheduled. It's difficuilt to keep an eye on what's released if we directly add commits to the main branch.

_Short-Lived_ branch:
* Typically for new features, bug fixes, refactorings, experiments....
* Mostly based on the main branch and then deleted after safely merged or rebased to the main branch
---
__GitHub Flow__
![[github-flow.png]]
very simple and lean structure: 1 main branch + short-lived branches

__GitFlow__
![[gitflow.png]]
more structured and more rules: main + develop + short-lived branches
* main branch is the reflection of current production state
* feature branches start from _develop_ and will be merged back into it
* when release, create a release branch and do the testing and bug fix and stuffs. if it's okay then merge it back to _main_ and add a tag for that release commit on _main_ and finally close the release branch.
---
## #3 Pull Requests
>It's not a git feature, but provided by the git hosting platform. So it is different in GitHub, git lab, Bitbucket, Azure DevOps, etc. Basic principles are all the same though.

What is _pull requests_?
1. After you finished working on a feature, without a pull request you'd simply merge your code into the main branch. But sometimes we want someone or people to look over the code before merging. This is what pull request is made for.
2. With pull requests, we can invite other people to review our work and give us feedback.
3. After reviewer approved then it is allowed to merge into another branch.

Another use case:
1. For example there is a popular open source repository, you want to improve something but you're not one of the main contributors and not allowed to push commits.
2. In this case we can fork the original repository.
>Fork is a personal copy of a git repository.
3. Then make changes in our forked version and open a pull request to include those changes into the original repository.
4. The main contributors of the original repository can then review the changes and decide to include them or not.

How to use? (__GitHub__)
1. Fork the open source repository, and then clone the project.
2. Create a branch and make changes and commit.
3. On github website, it detects your changes (your new branch) and asks you if you want to create a pull request. (because in fork environment this is what people mostly want to do)

Important:
* Pull requests are based on branches, not on individual commits.
---
## #4 Merge Conflicts
First of all we can undo a merge when conflict happens by using
``git merge --abort`` or ``git rebase --abort``
This is true even after we started resolving some of the conflicts. So we can always just return to a clean state and start over again when dealing with merge conflicts.
___
How does a merge conflict looks like?
![[git-merge-conflict.png]]
This is a simple example of a single merge conflict. In our current branch, HEAD, we changed lines 16-19. But in the develop branch, it deleted them.

Our changes are indicated under the  ``<<<<<<< HEAD`` section. Then the changes in _develop_ branch are separated by ``=======`` and ended with ``>>>>>>> develop``.
___
Okay then how to solve a conflict?

>Simply __clean up__ the file.

But sometimes we use merge tool to help visualizing things better:
* IDE like Visual Studio or Visual Studio Code
* Kaleidoscope app

What if I want to only keep my changes or their changes?
`git checkout --ours/theirs <filename>`
`git add <filename>`

After resolving just commit the changes.
___
## #5 Merge vs Rebase
How a Merge Works:
![[git-merge.png]]
1. first git looks for a common commit of the 2 branches it's called a __common ancestor__. in this commit both branches had the same content and after that they evolve differently.
2. then git looks for the end points of each branch.
3. In a simplest case that the branch to merge has no new commit. Git can simply add all the commits in the branch to be merged to the other branch. This is called a _Fast-Forward Merge_. ![[git-fast-forward-merge.png]]
4. In most cases though, both branches moved forward differently. In this case git will have to make a new commit which contains the differences between 2 branches. This commit is called the _Merge Commit_. ![[git-merge-realistic.png]]
5. If we want to understand what the merged branch is like after the merge commit. Just look at the commit history of both branches.
---
Why use rebase then?
* No merge commit. The commit history will look like a straight line. ![[git-rebase.png]]

How rebase works?
1. Git removed all commits that happened after the common ancestor commit in branch A. Actually more like saved or parked somewhere temporarily.
2. Commits from branch B are then applied to the branch.
3. Finally the parked commits are placed on top of the integrated commits from branch B.

Important:
* Rebase rewrites the commit history.
	* Here we can see the final step of the rebase, which apply the parked commit, C3 from branch A, back to the branch.
	* However, git does this by creating a new commit, C3*, with the same changes from C3. So they are in fact a different commit. ![[git-rebase-final-step.png]]
	* Before rebase, C1 is the parent of C3. After rebase, C4 is the parent of C3*.
* A commit contains properties like the author, date changed, and who its parent commit is. Changing any of these effectively create a new commit and a new commit hash.
* Rewriting commit history is not a problem for commits that havn't been published or pushed yet. But if it is already pushed, another developer might have based their work on the original C3 commit, which is not there anymore in the rebased commit history.

>Do NOT use Rebase, or rewrite commits, that you've already pushed on a remote repository!
>Only use them for cleaning up local commit history before merging it into a shared team branch.
---

