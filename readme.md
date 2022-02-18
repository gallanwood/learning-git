
# Git Notes


  - [Basic Commands](#basic-commands)
    - [Configuration](#configuration)
    - [Initialisation](#initialisation)
    - [Local Files](#local-files)
    - [Staging](#staging)
    - [Status and Diff](#status-and-diff)
    - [Commit](#commit)
    - [Log](#log)
    - [Alias](#alias)
    - [Delete and Reset](#delete-and-reset)
  - [Branching](#branching)
    - [Merge](#merge)
    - [Merge and Diff](#merge-and-diff)
    - [Stash](#stash)
  - [GitHub](#github)
    - [Create a repo - reminder](#create-a-repo---reminder)
    - [Create a remote](#create-a-remote)
    - [Push to Github](#push-to-github)
    - [Clone from Github](#clone-from-github)

## Basic Commands

### Configuration

  - `git` gives a basic help message and precedes all Git commands and starts the interactive Git CLI.
  - `q` exits the interactive Git environment returning to the system CLI
  - `git config --global` sets the Git *user*, *email* and optionally specifies a code *editor* application
  - `git config --global editor = 'code';` use 'code' for VSCode
  - `git config --global username = 'Gavin Allanwood';`
  - `git config --list` to view the global configuration
  - `git config --global core.excludesfile ~/Sites/.gitignore_global` configures Git to use a global exclude file, in this case located in the user's *Sites* directory

### Initialisation
- By default, Git works on all files in an initialised project directory, including existing and new files and sub-directories. You probably shouldn't initialise Git in a directory that is already included in another Git repository further up in the file heirarchy.
- `git init` initialises a Git directory within the current project path
- Subdirectorys within the project directory will be included in the repo
- VS Code hides the Git directory by default. Use `ls-la` to see it or change the `files-exclude` setting in Code to stop hiding the Git directory in the Explorer. This is not strictly necessary as access to the files is unlikely to be needed.

### Local Files
- Quickly create local files to practice Git
- `echo file some content > test.md` to quickly create a new file in the current working directory and write *some content* to the file
- `mkdir new_directory && cd $_ && echo some content > test.md` to create a new directory in the current working directory and change to it before creating a new file which has *some content*. This command will change your current working directory to the *new_directory*

### Staging
- `git add somefile.txt` stages a file in preparation for commiting it to Git
- Git does not keep a version of the file until it is actually *commited*
- `git add .` stages all modified files in one go unless they are excluded by an entry in a *.gitignore* file
- `echo somefile.txt > .gitignore` to add a file name to *.gitignore* which will be created if necessary

### Status and Diff
- `git status` lists files with their current Git status
- newly added files display as *new file*
- files that are staged and then edited prior to a commit are shown as *modified*
- `git add` a file directly before running commit if you want any changes to be commited, otherwise the repo will not be updated for that file.
- `git diff somefile.txt` shows any code differences between a staged file and the actual file saved in the project's workspace

### Commit

- `git commit` opens the editor for entering a commit message. The convention is to write the message in the present tense eg: 'fix the salary overpayment bug'
- `git commit -m 'correct the urls in home page'` is a quick way to include the commit message within the commit command
- Git keeps a version of all staged files when they are commited
- Remember to add files so that they become staged before running a commit
- `git commit --amend` overwrites a previous commit without creating a new one. Use only locally
- `git checkout --orphan new-master master` to create a branch that can be used to squash commmits. Follow with `git commit -m "fresh initial commit message"` and overwrite the old master branch reference with the new one `git branch -M new-master master`

### Log

- `git log` displays the commit hash, branch, author and date for each commit 
- `git log --oneline` displays the comment for every commit, one per line
- `git log --graph` displays logs organised as a graph of branches
- `git show 94ab7575ecf8d732d1f1e722aafad1ca625197b1` shows any changes made to a specific commit, identified by its hash

### Alias

- `alias l="git log --oneline"` creates a shortcut key alias for the Git command in double-quotes, in this case `l` will shortcut to `log --online`
- `git config --global alias.alias "config --get-regexp ^alias\."` creates a shortcut using the word `alias` to output a list of all aliases

### Delete and Reset
 
- `git reset --hard 94ab7575ecf8d732d1f1e722aafad1ca625197b1` to delete local commits in the active branch **and** the associated file changes using the hash reference **of the preceding commit**, found when running `git log`. The commits **subsequent** to the chosen commit hash are deleted. The commit that owns the hash in the command is retained as are all the commits in other branches.
- `--soft` deletes local commits in the active branch subsequent to the chosen commit hash while retaining subsequent file changes, returning all affected files to 'staged' status
- `git restore --staged somefile` to unstage a file in the active branch
- `git checkout -- somefile` to undo the 'modified' status of a file in the active branch and remove any saved changes *in the file* since the previous version commit. If the file is open in the editor, VS Code will prompt to overwrite the now newer file on save or cancel
- `rm -rf .git` inside the current **project** directory to nuke the Git directory and start again
- `git log` to confirm deletion

## Branching

- A Branch is an alternate codebase timeline created from an existing branch, usually the Master branch
- Isolated changes and additions can be made in a branch without affecting other branches
- A branch can be merged into the master when the changes are complete
- If conflicts occur between a branch and the Master they can be resolved during the merge
- `git branch` displays current branches.
- A new project will show `* [master]` branch. The asterisk means that it is the currently active branch
- `git branch -v` shows the most recent commit for every Branch
- `git checkout -b 'branch-name'` creates a new branch including all the tracked files in the current Git directory and switches to the new branch.
- `git checkout master` switches to the Master branch or any other (using tab will help autocomplete the name)
- When a branch is checked-out, local files change to match its content
- `git branch -d somebranch` to delete a branch

### Merge

- `git branch -v` to list branches
- Example when merging work done in a *dev* branch to the master:
- `git add .` and `git commit` in the *dev* branch
- `git checkout master` to switch to the master
- `git merge dev` to merge the *dev* branch with the Master
- Example when merging work done in the master with a *pages* branch to update a GitHub pages source:
- `git add .` and `git commit` in the master branch
- `git checkout pages` to switch to the *pages* branch
- `git merge master` to merge the master branch with the *pages* branch

### Merge and Diff

- When a branch is created from a master, the codebase upto that point is included in the branch
- Conflicts happen when editing creates differences to the same lines of code in each branch 
- Before a branch can be successfully merged, Git may ask you to 'fix conflicts and then commit the result'
- `git diff somefile` will show the differences between the same lines of code in each branch
- The file itself will identify the differences using markers that Git has inserted
- Delete the markers and the code that is redundant
- `git status` to identify the file
- `git add somefile` and `git commit .` to merge

### Stash

- `git stash` saves a working directory
- Files in the stash are ignored until they are unstashed
- Files can be unstashed to a different branch
  
  
## GitHub

### Create a repo - reminder

- `git init` inside a project directory
- `git add .` will add all the files in the project directory to Git
- `git status` to check that they are all there
- `git commit -m 'project first commit'` to make the first commit
- `git log` to check the first commit and `q` to exit

### Create a remote

- Create a new repo in GitHub with the same name as the local eg: 'project'
- `git remote add origin https://Github.com/gallanwood/project.Git` copied from the GitHub 'Quick Setup' page
- `git remote remove origin` if you need to remove a remote connection 

### Push to Github
- `git push -u origin master` pushes the local files to the repo. The `-u` flag sets the status to 'upstream'
- make a change to a **local** file and save it so that it displays the `M` flag in the VSCode file view
- `git add .`
- `git commit -m 'Change something'`
- `git push` to push the change to GitHub
- `git push --force` when the remote repo gets ahead of the local, eg: due to a `git commit --amend`
  
### Clone from Github

- Find the green `Code` button/dropdown in GitHub's repository view and copy the Clone link eg: `https://Github.com/gallanwood/Git-laravel-test.Git`
- `git init` inside a fresh directory, locally or on another computer
- `git clone https://Github.com/gallanwood/Git-laravel-test.Git` including the link after the command. Git will create a directory with the same name as the repo.
  






