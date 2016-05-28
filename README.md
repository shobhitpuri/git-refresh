## Index

1. [git-refresh](#git-refresh) - Refresh current local branch with any remote branch easily. 
2. [git-purem](#git-purem) - Push local branch changes to remote branch after updating from it.
3. [Setup Cutom Commands](#setup) - Get started in 5 minutes max.
4. [Creating Custom Git Commands](#writing-custom-git-commands) - Have your own ideas? Create custom scripts.

## git-refresh
This enables you to pull changes from a different remote branch to your local branch with just one command. 
You can write your own custom git commands to do whatever repetitive actions you do multiple times a day. 
For, me I usually update my local branch with the remote branch from which I branched off initially. 
Doing this would usually take multiple commands.

Instead of:
```
git stash
git checkout remote-branch
git pull --rebase origin remote-branch
git checkout current-branch
git rebase remote-branch
git stash apply
```
you can just do:
```
git refresh remote-branch-name
```

<b>Usage</b>
```
git refresh remote-branch-name
```
`remote-branch-name` is the remote branch from which you want to pull changes. 

## git-purem

git-purem, i.e (short form of git push remote) enables you to push your changes to remote origin to the same branch name as local. It also checks if the remote branch exists or not. If it exists, it updates your local branch with remote before pushesing the commits.

Instead of:
```
git stash
git pull --rebase origin current-branch
git push origin current-branch
git stash apply
```
you can just do:
```
git purem
```

<b>Usage</b>

While you are on your current local branch you want to push, do:
```
git purem
```

## Setup

1. Put the `git-refresh` file anywhere on your system. Lets say you've put in the folder named `gitScripts`, so the folder path is `/Users/username/path/gitScripts/`
2. Add the directory path to your environment `PATH`. For Linux/Mac, you can edit your `bash_profile` by doing `vim ~/.bash_profile`. Add following line in the file in the beginning:
   
   ```
   #!/bin/bash
   # For git commands
   export PATH=$PATH:/Users/user/Documents/gitScripts
   # Other existing export statements.
   # End of file
    ```
   Now after saving the file, do the following on the terminal:
   
   ```
   source ~/.bash_profile
   ```
3. That's it. You are done. You should be able run the command. 


## Writing custom git commands

Lets say you want to make a command `git awesome` which takes one parameter and then calls series of git commands. 

1. Create a file named `git-awesome` in a folder somewhere.
2. Add that folder path to your environment `PATH` as shown previously, if not already.
3. Have the following inside the file:

   ```
   #!/bin/sh

   # Check if params are sufficient enough to go ahead.
   # P.S: This command takes one parameter so check if you have the param.
   parameter=$1
   test -z $parameter && echo "ERROR: Please provide the param." 1>&2 && exit 1

   # Find which is your current branch
   if currentBranch=$(git symbolic-ref --short -q HEAD)
   then
     echo On branch $currentBranch
     echo Doing work using $parameter ...
     # Write your git commands here
     echo Success!
   else
     echo ERROR: Cannot find the current branch!
   fi
   ```

<i>Credits and Inspiration</i> : [Extending Git: add a custom command](http://blog.santosvelasco.com/2012/06/14/extending-git-add-a-custom-command/)
