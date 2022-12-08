# Git tutorial
## Introduction
We will learn git! This text will guide you through the basic concepts of git to get you started. We do not want to maintain a whole code-project for this tutorial, so we will simply use a bunch of text files, as a code basis, to demonstrate the fundamentals of git.

## Prerequisites:
- an account for bitbucket or github
- a working git installation
- you need to have a linux shell and should be familiar with it

## A bit of Theory
There are some git related terms you should be familiar with.


### Workspace
This is simply the project folder where your source files live. It also contains the .git folder. The workspace represents the folder of the project you’d like to put under version control.


### Index and Staging
The index is a snapshot of the workspace. It acts as a layer between your working directory and your local repository. Before you commit some changes into the local-repository, you first add (git add) them to the index. Adding changes to the index is called staging.

### Remote-repository
It contains all versioned files and changes. Usually it lives on a server (e.g. github.com or bitbucket.org). When you start working in an existing project you clone the remote-repository to your own machine into a local-repository. By convention the name of the remote-repository is “origin”, it reflects that this repository is the origin of your project.

### Local-repository
The local repository is simply a clone of the remote repository. When you commit changes in your workspace (git commit) they first get into the local repository, before you push them into the remote repository with git push.

### HEAD
head points to the most recent version of the current branch (this is master by default). We speak about a so-called detached HEAD if you checked out an older commit. You can see where HEAD points by look inside .git/HEAD or use the command git log

## Get familiar with the git environment
To get an first overview, which commands are provided by git you can use `git --help`
Let’s have a look which git version we have installed with `git version`

Cloning an environment from a remote-repository
Before you can start, you need to have a working git environment. Therefore we clone an existing project. Create a repository in bitbucket.org or github.com and clone it to your local filesystem. Therefore you go into a folder in which the root folder of the project should live and execute the git clone <repository-url> command. You find the repository-url after you created the repository in git or bitbucket. The command clone could look like this:

git clone git@bitbucket.org:sgbeck1981/githouse.git

A new folder with the repository name is created. The content of this (project) folder is now under version control. Inside the folder you will find a folder .git (this is where all the changes are stores) and eventually a .gitignore file (this file is for excluding files from being commited)

Commiting your first changes to the repository
Now, that you have a working environment under source control, we learn the commands to bring your changes into the repository. Go into your working directory and create a file, insert some text and save it. Now you will stage this specific file with the command:

git add <filename>

To stage all changes or files you could use a “.” instead of the filename:

git add .

Now that you have your changes in the Index it’s time to commit them to your local-repository. Commits always need to have a small description of what you’ve done.

git commit -m “Describe the change”

The commands git add and git commit will be used very often in a row. There is a shorthand to execute both of them at the same time:

git commit -a -m “Describe the change”

All our changes are in the local-repository now. The last step would be to push the changes from your local-repository to the remote repository.

git push


That’s it! All your changes are in the remote-repository.
Get changes from the repository
When you work in a team, other members will bring their changes also into the remote-repository. To stay up to date, you need to get thich changes into your local-repository. This can be achieved with the command:

git pull



Working with branches
Working with branches allows you to handle a set of commits separated from the main branch. When your changes are working, you can merge your branch into the master branch. First have a look which branches are existing in your environment:

git branch
The branch with the asterisk is the branch you are currently using. You can always create a new branch with:

git branch <branch-name>

When you’ve created a new branch you are not automatically using this branch. To switch to that branch you can use:

git checkout <branch-name>

There is a shorthand for create a new branch and switch to it:

git checkout -b <branch-name>

When you have finished your changes in the branch you can merge it into your master branch. Therefore you must be inside your master branch and execute this command:

git merge <name-of-branch-to-merge>

If you don’t need a branch any more, you can delete it with: (attention) by using:

git branch -d <branch-name>







Create a fresh project under git version control:
mkdir myproj
git init

Connect the local repository to a remote repository named origin
git remote add origin git@bitbucket.org:sgbeck1981/gittest.git

git branch --set-upstream-to=origin/master master
git pull origin master --allow-unrelated-histories

Show your remote repositories
git remote

removes changes that are not staged (git add)
git clean

removes all staged changes, that are not commited yet
git reset

reverts the last commit
git revert HEAD

checkout a new branch and switch to it
git branch new-branch
git checkout new-branch

or shorthand

git checkout - new-branch

