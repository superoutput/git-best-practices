# Git Best Practices
As you read through, remember that these workflows are designed to be guidelines rather than concrete rules. We want to show you what’s possible, so you can mix and match aspects from different workflows to suit your individual needs.

When evaluating a workflow for your team, it's most important that you consider your team’s culture. You want the workflow to enhance the effectiveness of your team and not be a burden that limits productivity. Some things to consider when evaluating a Git workflow are:
- Does this workflow scale with team size?
- Is it easy to undo mistakes and errors with this workflow?
- Does this workflow impose any new unnecessary cognitive overhead to the team?

## Centralized as origin
The repository setup that we use and that works well with this branching model, is that with a central “truth” repo. We will refer to this repo as **origin**

<!--
![Central repo as origin](./images/centr-decentr@2x.png "Central repo as origin")
-->
<p align="center">
    <img width="500" src="./images/centr-decentr@2x.png">
</p>

Each developer pulls and pushes to origin. But besides the centralized push-pull relationships, each developer may also pull changes from other peers to form sub teams. For example, this might be useful to work together with two or more developers on a big new feature, before pushing the work in progress to origin prematurely. In the figure above, there are subteams of Alice and Bob, Alice and David, and Clair and David.

Technically, this means nothing more than that Alice has defined a Git remote, named bob, pointing to Bob’s repository, and vice versa.

## Git Branch
The main idea behind the Git workflow branching strategy is to isolate your work into different types of branches. There are five different branch types in total:
- Master
- Develop
- Feature
- Release
- Hotfix

<!--
![Git Branching Model](./images/git-model@2x.png "Git Branching Model")
-->
<p align="center">
    <img width="500" src="./images/git-model@2x.png">
</p>

The two primary branches in Git workflow are master and develop. There are three types of supporting branches with different intended purposes: feature, release, and hotfix.

### The Primary Branches

<!--
![Primary Branches](./images/main-branches@2x.png "Primary Branches are master and develop")
-->
<img align="right" width="200" src="./images/main-branches@2x.png">

#### Master Branch
The purpose of the main branch in the Git workflow is to contain **production-ready code** that can be released. Other branches will be merged into the main branch after they have been sufficiently vetted and tested.

#### Develop Branch
The develop branch is created at the start of a project and is maintained throughout the development process, and contains pre-production code with newly developed features that are in the process of being tested.

Newly-created features should be based off the develop branch, and then merged back in when ready for testing.

### The Supporting Branches
#### Feature Branch

<img align="right" width="100" src="./images/fb@2x.png">

When working on a new feature, you will start a feature branch off the develop branch, and then merge your changes back into the develop branch when the feature is completed and properly reviewed.

#### Release Branch
The release branch should be used when preparing new production releases. Typically, the work being performed on release branches concerns finishing touches and minor bugs specific to releasing new code, with code that should be addressed separately from the main develop branch.

#### Hotfix Branch

<img align="right" width="250" src="./images/hotfix-branches@2x.png">

The hotfix branch is used to quickly address necessary changes in your master branch.
The base of the hotfix branch should be your main branch and should be merged back into both the main and develop branches. Merging the changes from your hotfix branch back into the develop branch is critical to ensure the fix persists the next time the main branch is released.

## Git Commmit Message
The contributors to these repositories know that a well-crafted Git commit message is the best way to communicate context about a change to fellow developers (and indeed to their future selves). A diff will tell you **what** changed, but only the commit message can properly tell you **why**.

***Re-establishing the context of a piece of code is wasteful. We can’t avoid it completely, so our efforts should go to reducing it [as much] as possible. Commit messages can do exactly that and as a result, a commit message shows whether a developer is a good collaborator.***

### Rules for a great git commit message style
* Separate subject from body with a blank line
* Do not end the subject line with a period
* Capitalize the subject line and each paragraph
* Use the imperative mood in the subject line
* Wrap lines at 72 characters
* Use the body to explain what and why you have done something. In most cases, you can leave out details about how a change has been made.

### Information in commit messages
* Describe why a change is being made.
* How does it address the issue?
* What effects does the patch have?
* Do not assume the reviewer understands what the original problem was.
* Do not assume the code is self-evident/self-documenting.
* Read the commit message to see if it hints at improved code structure.
* The first commit line is the most important.
* Describe any limitations of the current code.
* Do not include patch set-specific comments.

Details for each point and good commit message examples can be found on https://wiki.openstack.org/wiki/GitCommitMessages#Information_in_commit_messages

### References in commit messages
If the commit refers to an issue, add this information to the commit message header or body. e.g. the GitHub web platform automatically converts issue ids (e.g. #12345678) to links referring to the related issue.

In header:
```
    [#12345678] Refer to GitHub issue…
```
In body:
```
    Switch libvirt get_cpu_info method over to use config APIs

    The get_cpu_info method in the libvirt driver currently uses
    XPath queries to extract information from the capabilities
    XML document. Switch this over to use the new config class
    LibvirtConfigCaps. Also provide a test case to validate
    the data being returned.

    DocImpact
    Closes-Bug: #12345678
    Implements: blueprint libvirt-xml-cpu-model
    Change-Id: I4946a16d27f712ae2adf8441ce78e6c0bb0bb657
```

#### Note
- Closes-Bug: #12345678 -- use 'Closes-Bug' if the commit is intended to fully fix and close the bug being referenced.
- Partial-Bug: #12345678 -- use 'Partial-Bug' if the commit is only a partial fix and more work is needed.
- Related-Bug: #12345678 -- use 'Related-Bug' if the commit is merely related to the referenced bug.

## For All the Routine Commands
### Show the working tree status
```
git status
```

### List All Branches
- To see **local branches**
```
git branch
```
- To see **remote branches**
```
git branch -r
```
- To see all local and remote branches
```
git branch -a
```
**NOTE:** The current local branch will be marked with **(*)**

### Create a New Branch
```
git checkout -b my-branch-name
```
**NOTE:** Replacing my-branch-name with whatever name you want

### Switch to a Branch from Local
```
git checkout my-branch-name
```

### Switch to a Branch from Remote
```
git pull
git checkout --track origin/my-branch-name
```

### Push to a Branch
- If your local branch **does not exist** on the remote, run either of these commands
```
git push -u origin my-branch-name
git push -u origin HEAD
```
**NOTE:** HEAD is a reference to the top of the current branch, so it's an easy way to push to a branch of the same name on the remote. This saves you from having to type out the exact name of the branch!
- If your local branch **already exists** on the remote, run this command
```
git push
```

### Merge a Branch
1. You'll want to make sure your working tree is clean and see what branch you're on. Run this command
```
git status
```
2. First, you must check out the branch that you want to merge another branch into (changes will be merged into this branch). If you're not already on the desired branch, run this command
```
git checkout master
```
**NOTE:** Replace master with another branch name as needed.
3. Now you can merge another branch into the current branch. Run this command
```
git merge my-branch-name
```

### Delete Branches
- To delete a **remote branch**
```
git push origin --delete my-branch-name
```
- To delete a **local branch**
```
git branch -d my-branch-name
```
**NOTE:** The **-d** option only deletes the branch if it has already been merged. The **-D** option is a shortcut for --delete --force, which deletes the branch irrespective of its merged status.

### Check Project Repository History
```shell
$ git log --graph --all --oneline --decorate
```
or
```shell
$ git log --pretty=oneline --abbrev-commit
```

### Undo a commit & redo
```shell
$ git reset HEAD~                              # (1)
[ edit files as necessary ]                    # (2)
$ git add .                                    # (3)
$ git commit -c ORIG_HEAD                      # (4)
```
1. `git reset` is the command responsible for the undo. It will undo your last commit while leaving your working tree (the state of your files on disk) untouched. You'll need to add them again before you can commit them again).
2. Make corrections to working tree files.
3. `git add` anything that you want to include in your new commit.
4. Commit the changes, reusing the old commit message. `reset` copied the old head to `.git/ORIG_HEAD`; `commit` with `-c ORIG_HEAD` will open an editor, which initially contains the log message from the old commit and allows you to edit it. If you do not need to edit the message, you could use the `-C` option.

**Alternatively, to edit the previous commit (or just its commit message),** `commit --amend` will add changes within the current index to the previous commit.

**To remove (not revert) a commit that has been pushed to the server,** rewriting history with `git push origin main --force[-with-lease]` is necessary. It's almost always a bad idea to use `--force`; prefer `--force-with-lease` instead.

`HEAD~` is the same as `HEAD~1`. The article [What is the HEAD in git?](https://stackoverflow.com/a/46350644/5175709) is helpful if you want to uncommit multiple commits.

### Local remove commit
```shell
$ git reset --hard <commit-hash>
```
Or delete second commit line and save it
```shell
$ git rebase -i HEAD~2
```

### Remote remove commit
```shell
$ git push -f origin branch
```
git push --force <remote> <commit-ish>:<the remote branch>
```shell
$ git push --force origin 606fdfaa33af1844c86f4267a136d4666e576cdc:master
```

If you get the error:
*error: Your local changes to the following files would be overwritten by checkout:*
...
Please commit your changes or stash them before you switch branches

Then you can stash your work, create a new branch, then pop your stash changes, and resolve the conflicts:
```shell
$ git stash
$ git checkout -b branch_name
$ git stash pop

$ git reset --soft <commit-id>
$ git stash save "message"
$ git reset --hard <commit-id>
$ git stash apply stash stash@{0}
$ git push --force

$ git merge <commit-id>
```
Delete remote branch:
```shell
$ git push -d origin <branch_name>
```

### Change committer name/email
1. Fix the git configuration (local) :
```shell
$ git config --global user.name "Your-name"
$ git config --global user.email "your@email.com"
```
2. Rewrite commit history :
#### Option 1
Use --amend for the **Very Last Commit** :
```shell
$ git commit --amend --reset-author
# Or
$ git commit --amend --author="Your-name <your@email.com>"
```
#### Option 2
Use git filter-branch **for All Ealier Commit**, run this the script *[git-filter.sh](./scripts/git-filter.sh)* :
```shell
$ git filter-branch -f --env-filter '
WRONG_EMAIL="wrong@email.com"
NEW_NAME="Your-name"
NEW_EMAIL="your@email.com"

if [ "$GIT_COMMITTER_EMAIL" = "$WRONG_EMAIL" ]
then
    export GIT_COMMITTER_NAME="$NEW_NAME"
    export GIT_COMMITTER_EMAIL="$NEW_EMAIL"
fi
if [ "$GIT_AUTHOR_EMAIL" = "$WRONG_EMAIL" ]
then
    export GIT_AUTHOR_NAME="$NEW_NAME"
    export GIT_AUTHOR_EMAIL="$NEW_EMAIL"
fi
' --tag-name-filter cat -- --branches --tags
```
#### Option 3
Use Rebase **for All Ealier Commit** :
```shell
$ git rebase -i HEAD~1
```
An editor window pops up where the commit is marked as *pick*, change it to *edit* and save.
Correct the author information :
```shell
$ git commit --amend --reset-author
```
Then continue to the next concerned commit :
```shell
$ git rebase --continue
```
3. Finally, push to the remote repository :
```shell
$ git push --force
```

### Git extension for versioning large files
#### Download Git Large File Storage
**Download:** [https://git-lfs.github.com/](https://git-lfs.github.com/)
**Homebrew:** `brew install git-lfs`
**MacPorts:** `port install git-lfs`

1. Download and install the Git command line extension.
```shell
$ git lfs install
```
2. Use Git LFS to configure additional file extensions (or directly edit your .gitattributes).
```shell
$ git lfs track "*.bin"
```
Now make sure .gitattributes is tracked:
```shell
$ git add .gitattributes
```
3. Commit and push to GitHub as you normally would
```shell
$ git add file.bin
$ git commit -m "Add large binary file"
$ git push origin main
```

## Credits
- [Comparing Workflows](https://www.atlassian.com/git/tutorials/comparing-workflows)
- [Git Concepts - Quick Start Guides](https://www.gitkraken.com/learn/git/git-flow)
- [Git Best Practices - Git Branch](https://www.gitkraken.com/learn/git/best-practices/git-branch-strategy) *(Biased comparations & Useless, just keep it for a while)*
- [A successful Git branching model](https://nvie.com/posts/a-successful-git-branching-model/) *(Over 10 years conceived model)*
- [How to Write a Git Commit Message](https://chris.beams.io/posts/git-commit/)
- [Git Commit Good Practice](https://wiki.openstack.org/wiki/GitCommitMessages) *(Best of the Best Examples)*
- [Commit Message Guidelines](https://gist.github.com/robertpainsi/b632364184e70900af4ab688decf6f53)
- [How can I change the author name / email of a commit?](https://www.git-tower.com/learn/git/faq/change-author-name-email/)
- [How to change a Git commit message after a push](https://www.educative.io/edpresso/how-to-change-a-git-commit-message-after-a-push)

## Other Links
- [x] https://github-readme-stats.vercel.app/api/top-langs/?username=superoutput&layout=compact&langs_count=7&hide=Jupyter%20Notebook
- [x] https://github.com/frohoff/ysoserial
- [x] https://github.com/for-GET/know-your-http-well/blob/master/methods.md
- [x] https://medium.com/20scoops-cnx/github-workflow-from-scratch-99b07e8c318b
- [x] https://www.nobledesktop.com/learn/git/git-branches
- [x] https://stackoverflow.com/questions/8981194/changing-git-commit-message-after-push-given-that-no-one-pulled-from-remote
- [ ] https://opensource.com/article/20/7/git-best-practices
- [ ] https://docs.gitlab.com/ee/topics/gitlab_flow.html
- [ ] https://www.freecodecamp.org/news/writing-good-commit-messages-a-practical-guide/
- [ ] https://medium.com/@mingloan/managing-a-better-git-workflow-556281520e1a
- [ ] https://www.cloudbees.com/blog/branching-strategy/
- [ ] https://www.saladpuk.com/basic/git/branching-strategy
- [ ] https://devconnected.com/how-to-push-git-branch-to-remote/
- [ ] https://www.atlassian.com/git/tutorials/comparing-workflows
- [ ] https://guides.github.com/introduction/flow/
- [ ] https://github.com/andela/bestpractices/wiki/Git-Workflow
- [ ] https://github.com/asmeurer/git-workflow
- [ ] https://gist.github.com/vlandham/3b2b79c40bc7353ae95a
- [ ] https://medium.com/@rieckpil/30-minutes-every-day-for-your-craft-committing-code-to-github-for-365-consecutive-days-eec8b73b5105
- [ ] https://medium.com/mop-developers/the-subtle-discipline-of-daily-commits-f91136f0ed01
- [ ] https://github.com/trein/dev-best-practices/wiki/Git-Commit-Best-Practices
- [ ] https://sethrobertson.github.io/GitBestPractices/
- [ ] https://stackoverflow.com/questions/53213341/git-commit-daily-or-commit-when-feature-is-completed
- [ ] https://stackoverflow.com/questions/3817967/correct-git-workflow-for-shared-feature-branch
- [ ] https://www.freecodecamp.org/news/lessons-from-my-month-long-github-commit-streak-b8f3167d34ac/
- [ ] https://stackoverflow.com/questions/804115/when-do-you-use-git-rebase-instead-of-git-merge/804178#804178