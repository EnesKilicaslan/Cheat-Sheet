# Git Cheat Sheet

Git allows groups of people to work on the same documents (often code) at the same time, and without stepping on each other's toes. It's a distributed version control system.

------------------------

***Directory:***
A folder used for storing multiple files.

***Repository:***
A directory where Git has been initialized to start version controlling your files.


type **git init** to initialize a Git repository.

type the **git status** command to see what the current state of our project is.

------------------------

**staged:**
Files are ready to be committed.

**unstaged:**
Files with changes that have not been prepared to be committed.

**untracked:**
Files aren't tracked by Git yet. This usually indicates a newly created file.

**deleted:**
File has been deleted and is waiting to be removed from Git.

------------------------

**Staging Area:**
A place where we can group files together before we "commit" them to Git.

**Commit**
A "commit" is a snapshot of our repository. This way if we ever need to look back at the changes we've made (or if someone else does), we will see a nice timeline of all changes.

------------------------

***add all: You can also type git add -A . where the dot stands for the current directory, so everything in and beneath it is added. The -A ensures even file deletions are included.***

***git reset: You can use git reset <filename> to remove a file or files from the staging area.***



we can add all the txt new files using a wildcard with git add. Don't forget the quotes! ***git add '\*.txt'***

Shell wildcard expansion does not recurse into subdirectories. The wildcard is expanded before Git gets a chance to see it.

***If you use git add '\*.js', then Git will see the wildcard and will match it against all path names that end in .js. Because the asteriks is in the initial position, this will end up recursively adding all .js files including in subdirectories.***

------------------------

To see more information for each commit. You can see where new files were added for the first time or where files were deleted.
```sh
git log --summary
```

To see only the commits of a certain author:
```sh
git log --author=bob
```

Or maybe you want to see an ASCII art tree of all the branches, decorated with the names of tags and branches: 
```sh
git log --graph --oneline --decorate --all
```

To See only which files have changed: 
```sh
git log --name-status
```

------------------------

If you have not cloned an existing repository and want to connect your repository to a remote server, you need to add it with
```sh
git remote add origin <server>
```

Git doesn't care what you name your remotes, but it's typical to name your main one **origin**.
To push our local repo to the GitHub server we'll need to add a remote repository, like following
```sh
git remote add origin htt://github.com/try-git/try_git.git
```

The push command tells Git where to put our commits when we're ready. The name of our remote is origin and the default local branch name is master. The -u tells Git to remember the parameters, so that next time we can simply run git push and Git will know what to do.
```sh
git push -u origin master
```

------------------------

When we work with others and they made commits and pushed to github, we can pull down new changes by running.
```sh
git pull origin master
```

**git stash: Sometimes when you go to pull you may have changes you don't want to commit just yet. One option you have, other than commiting, is to stash the changes.**

Use the command ***'git stash'*** to stash your changes, and ***'git stash apply'*** to re-apply your changes after your pull.

The ***HEAD*** is a pointer that holds your position within all your different commits. By default HEAD points to your most recent commit, so it can be used as a quick way to reference that commit without having to look up the SHA.

take a look at what is  different from our last commit by using the git diff command. In this case we want the diff of our most recent commit, which we can refer to using the HEAD pointer. So, ***git diff HEAD*** to see the changes you just staged, run git diff with the --staged: ***git diff --staged***

You can unstage files by using the git reset command

***git reset octofamily/octodog.txt***

------------------------

To create a new branch named "feature_x" and switch to it using
```sh
git checkout -b feature_x
```

To delete the branch again
```sh
git branch -d feature_x
```




**Useful links:**

- [The Simple Guide](http://rogerdudler.github.io/git-guide/)
- [A Visual Git Reference](http://marklodato.github.io/visual-git-guide/index-en.html)
- [Think like git](http://think-like-a-git.net/)
- [Github Help](https://help.github.com/)








