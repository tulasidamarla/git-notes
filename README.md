# git-notes

Git is a distributed Version control system.

What is the difference between central and distributed VCS?
Ans: In central VCS, server is located at a central location. If the server is offline, we can able to see the files that are checked out, no additional information about the central repository. 
If something happens and central VCS gets corrupt, it needs a backup to recover. In Distributed VCS, everyone has a local repository. i.e. your local repository has all of the information that your 
remote repository has based on the last synchronized state. If something happens to the remote repository, every developer has a copy of that remote repository.

Git configuration setup
-----------------------
Once git is installed, from the command line we can check the version using the command:
git --version

We need to setup git account first by signup at https://github.com. Post that configure the account from the command line, so that we can start working with remote repository.
Here are the basic commands for configuration.

git config --global user.name "${yourusername}"
git config --global user.email "${youremail}"

To view the list of global configurations,

git config --list

For any help with git commands we can use git help. For example, with config we can use either

git help config (or) git config --help

Note:
With git we need to work with four states.
1)Working Directory.
2)Staging Area. (This is the area where we want to pick and choose what we want to commit or delete)
3)Committed files(.git directory)
4)Push to remote.


Pusing existing code to remote repository through the command line
------------------------------------------------------------------
First create a new repository on github.com and don't add any README or .gitignore files. These might cause some errors. Post that use the following commands.

git init (#This creates a .git directory, which contains everything related to the repository. If this directory is removed, our project will not be tracked by git anymore)
git status (#This shows list of all untracked files.)
git add --all (#To add all files to the staging area. Files added to staging are all tracked. If you want to ignore any metadata files, create a .gitignore file and add them)
git commit -m "first commit" (#Commit files to the local repository)
git remote add origin https://github.com/tulasidamarla/providerdataapp.git
git push -u origin master

Unstage
-------
To unstage files use the following command:

git reset <filename>.

Note: If don't pass filename with reset command, it will unstage all files. 

Commit logs
-----------
To view the commit logs, use the command "git log".

Checking out existing repository and working on it
--------------------------------------------------
Git clone command is used to checkout the project to local path. Here is the command:

git clone <remote_repository_url> <local_path>

To view the information about remote repository
-----------------------------------------------
git remote -v


To view the code changes made
-----------------------------
git diff

Working with branches
---------------------
To create a branch
------------------
git branch <branch_name>

To switch to a different branch
-------------------------------
git checkout <branch_name>

To view the information about branches
--------------------------------------
git branch -a 

Note: This command lists all local and remote branches. If we want to see only local branches, remove the flag "-a".

To push the locally created branch to remote 
--------------------------------------------
git push -u origin <branch_name>

Note: The flag "-u" is to associate local branch with the remote.

Merge the branch with master
----------------------------
1)Switch to master branch using "git checkout master".
2)Pull the code "git pull origin master" or simply "git pull".
3)To view already merged branches "git branch --merged" (#The new branches which are not merged yet, will not be displayed).
4)To merge "git merge <branch_name>.
5)Push the master after successful merge in local (i.e. git push origin master).

To delete the branch once merging is done
-----------------------------------------
1)Verify if the branch is in merged branches list ("git branch --merged")
2)To delete (git branch -d <branch_name>
3)Push the changes to remote because remote repository contains this branch. ("git push origin --delete <branch_name>")

Working with git stash
----------------------
Git stash is very handy whenever we have some changes but not yet ready for commit or sometimes you make your changes in wrong branch.

To save the changes made 
------------------------
git stash save "stash comment"

To see the list of all stashes
------------------------------
git stash --list

Note: The above command gives output similar to the below statement, which contains stash number, branch and the stash comment.
stash@{0}: On dev: refactoring code

To get the changes saved with stash
-----------------------------------
git stash apply <stash_number> (For ex "git stash apply stash@{0}")

Note: The above command won't remove the stash from stash list. If we want to remove from the stash list use "git stash pop" command instead of "git stash apply". 
Pop doesn't need any stash number because it will pop-up the top stash in the list.

To remove the stash
-------------------
git stash drop <stash_number>

Note: To remove all stashes "git stash clear".

***
Note: One good usecase of Stash is when you make changes by mistake in master branch and you try to switch the branch. Then git shows an error message that there are uncommitted changes.
You cannot commit these changes because you are in the master brancn. So you can stash them and switch the branch and can get these changes in the other branch.

To modify the commit message
----------------------------
git commit --ammend -m <new_message>

Missed files to a commit
------------------------
If we missed some files and want them to be added to the last commit,
git commit --amend

Note: The above command will display an interactive editor, where in which we can change the command message if we want.

***
If we want to see the last of files modified as part of a commit
-----------------------------------------------------------------
git log --stat

To undo changes to a specific file
----------------------------------
git checkout <filename>

Difference between git reset and git checkout
---------------------------------------------
If we have two branches, 'master' and 'develop' pointing at different commits, and we're currently on 'develop' (so HEAD points to it) and 
if we run git reset master, 'develop' itself will now point to the same commit that 'master' does.

If we instead run git checkout master(switch branch), 'develop' will not move, HEAD itself will. It means, HEAD will now point to 'master'.

In both cases we're moving HEAD to point to commit A, but how we do so is very different. 
reset will move the branch HEAD points to, checkout moves HEAD itself to point to another branch.

Cherry Pick
-----------
If we accidentally commit changes to a wrong branch, to move those commits to the required branch, we can use cherry pick command. Here is the process.
1)Choose the first 6/7 characters in the hash number of the commit using "git log" command.
2)Switch to the required branch using "git checkout <branch_name>
3)git cherry-pick <hash_number_copied_from_wrong_branch>

Note: cherry-pick command brings the commit to the current branch. We can see that using "git log" command again.

Note: To uncommit from the wrong branch, go to the branch and run the "git reset" command.

Reset
-----
There are 3 types of reset commands available.
1)soft: If we use the command to move HEAD to a older version using "git reset --soft <hash_number_of_commit>", It takes the current branch to specified commit.But all the files 
committed in the newer commits are kept in staging area. i.e. soft won't delete any files in the latest commit. It keeps them in staging area. We can verify using "git log".
2)mixed: This is the default reset command. If we use the same command above without the flag soft, HEAD is pointed to the specific commit. But, all the tracked files in the latest commit are
kept in the work tree, not in the staging area. We need to stage them using "git add" if needed.
3)hard: If we use the command "git reset --hard <hash_number_of_commit>", will move the HEAD to the specific commit. All, the tracked files are taken back to the specified commit. 
No changes to the tracked files after the commit can be seen.

*****
Note: Even with hard reset, any untracked files after the commit are still present.

To remove any untracked files
-----------------------------
git clean -df (where d is for directories and f is for files). 

Note: The above command is very handy, especially when we extract a zip file by mistake in the project directory, cleaning them is easy.

git reflog
----------
git reflog shows list of all commits when we referred them. It could show commits upto 90 days bydefault. It is possible to revert to a specific commit even after hard reset if 
it is available in reflog. From the reflog, We can checkout any previous commits. But, this could cause a detached head state. 

What is detached head?
Ans:With the "git checkout" command, you determine which revision of your project you want to work on. Git then places all of that revision's files in your working copy folder.
Normally, you use a branch name to communicate with "git checkout": For ex, git checkout dev.

However, you can also provide the SHA1 hash of a specific commit instead: For ex, "git checkout 56a4e5c08". 

This exact state - when a specific commit is checked out instead of a branch - is what's called a "detached HEAD".

The problem with a detached HEAD
--------------------------------
The HEAD pointer in Git determines your current working revision (and thereby the files that are placed in your project's working directory). Normally, when checking out a proper branch name, Git automatically moves the HEAD pointer along when you create a new commit. You are automatically on the newest commit of the chosen branch.

When you instead choose to check out a commit hash, Git won't do this for you. The consequence is that when you make changes and commit them, these changes do NOT belong to any branch.
This means they can easily get lost once you check out a different revision or branch: not being recorded in the context of a branch, you lack the possibility to access that state 
easily.

If we want to save these changes, create a branch from it and save. For ex, we can direcly use "git checkout -b test-branch 56a4e5c08" (or simply "git branch test-branch" from the detached state)

git revert
----------
using "git revert <hash_number>" we can revert back to a specific commit. All the other users, who have pulled the latest can see the changes in the history. So, history will be intact.
