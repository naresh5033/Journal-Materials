# Git commands (FCC)

1. the Perfect Commit

- add the right changes

  - dont't cram everything into a single commit, its a bad thing
  - the more complex or the more bigger the topics then its harder to understand.
  - so git stagging area will be really helpful in this context, which will allow us to specific files or even the parts of the file for the next commit.

- the golden rule of the version control is only combine the changes from the same topic in a single commit.
- to add the specific file `git add style.css`
- and to look the changes of the files what different it contains
  - `git diff index.html` lets say this file has 2 changes and the first one belongs to this commit and the second one belongs to the next commit.
  - we can add the first to the staging area, `git add -p index.html` the p is the patch level where we decide what to include and what not to.
  - and we can select the y for the first one and the n for the second one, so we can add the second one for the next commit.

2. the great commit message

- subject - the concise summary of what happend
- then the body, more detailed explanation
  - what is now different that b4.
  - what is the reason for the changes
  - Is there anything to watch out for or the anything particularly remarkable
  - `git status` .. `git commit` for the editor window . if we add a empty line after the subject git know its the body of the commit.
  - and `git log` to see the commit logs .

## Branching Strategies

- A written convention, agree on the branching workflow with the team.

1. git allows us to create the branches, but it doesn't tells how to use them
2. it highly depends on the project/ team size and how to handle them

- integrating the changes and structuring the releases.
  1. main development (always be integrating)
  2. state, release and the feature branches.

1. main development - few branches, relatively small commits, high quality testing and QA standards.
2. state, release and the feature branches. - different types of branches, fulfill diff types of jobs.

- Long running branches and the short lived branches are - two main types of branches

  - every git repo has atleast one long running branch called main or master.
  - they exist thru the lifecycle of our project
  - often they mirror stages in our dev life cycle.
  - the integration branch are often named develop or staging.
  - common convention - no direct commits to the branches. - commit are thru merge or rebase.

- Short lived branches - based on the long lived branch

  - for the bug fixes, refactor, new features, and experiments.
  - will be deleted after the integration, (merge/rebase)

- Two ex branching strategy
  - github flow and git flow

## GitHub Flow

- has only one long runnig branch, main branch + feature branches.
- "GitFlow" - more structured and more rules.
- Long running - main + develop
- short lived - featured, bug fix and releases

## Pull Requests

- the PRs are not the core features, they are provided by the git hosting platforms means they work differently on the github, gitlab, bit bucket, az devops etc
- we need prs to commmunicating about and the reviewing code.
- The PRs are always based on the branches not on the individual commits.
- the perfect ex is when we re working on a feature w/o the pr we'd merge the our code into main,
- when specially our code are bit more complex, we might wana ve a second pair of eyes look over our codes. and this is exactly what the pr are made for.
- the prs invite other people to review and provide feedback before merging,

- and the other use case is, its a way to contribute the code to other repositories, which we might not ve access to (ex- the open source repo, we re not the main contributors, and we re not allowed to push commits to the repo)
- another use case, fork in the connection

## Fork

- fork is our personal copy of the git repo
- ex - the open source repo.. we can fork the original repo and make changes in our forked version and then open the pr to include those changes into the original repo.
- then the contributors can review our changes and decides whether to include our changes or not
- `git branch test` to create a new branch, and to switch to the branch `git checkout test`
- once we made the changes to the branch, and commited the changes, we can push it to our own remote repo/our fork repo, `git push --set-upstream origin test `
- after push to the forked repo, if we go the repo we can see the github detects our changes in the forked repo and it will asks us if we want to make a PR for those changes.

## Merge Conflicts.

- when they happen ?

  - it can occurs when we integrate the merge changes from different sources,
  - integration is not only limited to merging branches, but conflicts can also happens when rebasing, or interactive rebasing when performing a cherry pick or pull, even when reapplying a stash and all of these actions performed some kind of integration and thats when the merge conflicts occurs.
  - `git merge, git cherry pick, git rebase, git pull, git stash apply `

- how to know when the conflict is occured ?
  - git will tell us when that occured
- how to undo a conflict and start over from again

  - `git merge --abort ` and `git rebase --abort `

- how to solve the Conflicts ?

  - may be we can use the desktop gui (like tower),
  - and we can ve some dedicated merge tools.. we can config the tool of choice using the git config command, and then in case of conflicts we can simply type git merge tool and ve it open for the conflict ex - "Kaleidoscope"

- `git mergetool`

## Merge vs Rebase

- lets see about integrating the branches getting our new code back to the existing branch.
- this merge and rebase are the two common ways to do that.
- when git performs for the merge it looks for the 3 commits,

  1. common ancestor commit (if we follow the history of 2 branches in a project, they always ve a atleast one commit in common)
  2. the other 2 commits are the endpoints of each branch,

  - combining these 3 commits we will perform the integration that we re aiming for.

- fast forward merge - the git can add all the commits from branch B on top of the common ancestor.
  and get the simplest form of the integration is called the fast forward merge.
- where both branches then share the exact same history

- "Merge Commit " is little different, as it is not created by developers instead it will developed by the git itself.
- and also it doesn't wrap the set of related changes, its purpose is to connect the 2 branches just like the knot.

## REbase

- knowing the pros and cons of what it does and doen't is always helpfull
- the rebase is just like the straight line of commits, `git rebase branch-B`
- first look at the BTS, - the git will remove all the commits on the branch A that happened after the ancestor commit, it will not throw away, we can think of it as parked or saved temporarily.
- then git applies new commit to the branch B, and at this point both branches temporarily looks exactly like the same.
- then in the final stage those parked commits needs to be included, the new commits on the branch A, they re posistioned on the top of the integrated commits from branch B. they re rebased.
- and as a result look like the development has happened in a straight line.
- there is no merge commit that contains all the combined changes, we preserve the original commit structure,
- one more important thing about the rebase is that it rewrites all the commit history.

## warnig notice

- the simple rule is to
- do not use the rebase on commit that we ve already been pushed or shared on a remote repo.
- instead use it for cleaning up the local commit history b4 merging it to the shared team branch.

# Interactive Rebases

- a tool for optimizing and cleaning up the commit history

- in short the interactive rebases will allow us to

  - change a commit message
  - delete commits
  - reorder the commits
  - combine multiple commits into one
  - edit/split an existing commits into multiple new commits
  - `git rebase -i HEAD~3 ` for the interactive rebase of the 3 rd head commit. and it will open the editor window and we will tell the git to what kind of manipulation we wana make. (we don't change any commit messages here.)
  - and in that window we can see all the action keywords. like reword and bunch of others
  - once we done and close the window, we will get one more window open, and now here is where we can finally make the commit changes.
    and the "squash" kw, we can make the two commits and combine into one.

  - `git log --oneline `.. if we wanna change the most recent commit, we can use `git commit ammend` .. if anything other than that, then we ve to use the interactive rebase
  - the first step of our interactive rebase determine the base commits
  -

## Cherry Picking

- normally when we integrate the commits, we do this on the branch level.
- integrating single specific commits.
- cherry picking allows us to select the individual commits to be integrated, ex - we can integrate only C2 not the C4.
- one thing to keep in mind, our main way of integrating will still be in the branch level.
- merge and rebase were built exactly for this job.
- we need to ve a good reason to use, its not a replacement for the merge and rebase.
- `git cherry-pick 3800..f` the hash of the commit -`git checkout master` and then `git reset --hard HEAD~1` its like deleting the commits from the master branch.

## Reflog

- we can thik of it as a diary, or the journel where the git logs every moments of the head pointer.
- its like when we commit, merge rebase, checkout, rebase, and reset all of the actions are documented here.
- lets see a perfect use case for the reflog
  - Recovering the deleted commits
  - lets say we wana get rid of the some commits, and then we use git reset, then we notice it was a bad idea ,and we panics
  - `git reflog` - will opens the reflog, the entries are chronologically ordered means the most recent/newest ones on the top.
- another use case with the reflog is the recovering deleted branches.
  - but incase if we re using the gui, like tower we can use the cmd+z to bring back the deleted branch.

## Submodules

- often times we use the 3rd party libs
- copy and pasting the 3rd party code has ugly downside.
- 1. mixing the external code with our own files (cause the lib is a project itself and we ve to separate it from our own work)and 2. updating the external code is again a manual process.

- the submodules is actually a git repository, except its nested inside the other parent repository.
- the submodules is fully functional git repository, we can merge, push pull or whatever we do with the git repository.
- `git submodule add https://..`
- now the actual content of the submodule aren't stored in our parent repository.
- the parent repo only stores the remote url or the sub, the local path inside the main project and the checkout revision.
- in the .gitmodules we can see the remote url and the path (local) `cat .gitmodules`
- also in the .gitconfig file `cat .gitconfig`
- and finally the git also keeps the copy of the each submodules git repo in its internal git modules folder.

- if we clone a repo with the submodules in it. we can see after we cloned there won't be any submodules content inside it.
- as we know that the parent will not ve the submodules content, it only ve its config files.
- so when we clone a project with the default git clone cmd we re only downloading the project itself with the configurations, the submodules folder however they will stay empty. to fix that \
- `git submodule update --init --recursive `

- if we think like the git clone and then the extra command is bit unnecessary, we can achieve this same with the option with git clone recurse submodules options.

- this tells the git to initialize the submodules when the cloning is finished automatically.
- `git clone --recurse-submodules https://...`

- "the checkout Revision in the standard git repository"

  - in a normal git repository we checkout the branch and automatically the last commit in the branch is our check out revision
  - and if we add new commit to the branch the check out revision is always automatically the newest commit.
  - the submodules repository on the other hand are always checked out on a specific commits not on a branch.
  - and it makes sense coz the content of the branch can change overtime, when the new commits arrive, and in a submodule repository we don't necessarily want that coz they re library code.
  - and in that situation we ve the guaranteed that we always had the specific revision of the code.

- so the submodules git repo is checkout on a specific commit, when the new commits happens the checkedout commit doesn't change.
- managing the submodules repo is really complicated, we can ve some gui like tower to maintain them/

## Search and Find

- since the git repository has all the logs of all the changes that ever made/
- sometimes we wanna browse thru this logs, lets see how the searching and the finding works

- filtering our commit history by

1. by date --before/ --after
2. by message --grep
3. by author --author
4. by file --<filename
5. by branch <branch-A>

- `git log --after="2019-7-2" ` all the commits that were made after that date, we can also combine the before flag here.
- `git log --grep="any specific message"` the commits that were made with the specific message
- lly we can look for the certain author with the author flag
- we can search for the certain files, and it can be useful for understand how the certain file evolved over the time. `git log -- README.md `
- finally wana see the commit in one branch and not in the other branch `git log feature/login..main` will show the commits in the main but not in the feature/login.
