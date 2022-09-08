# Creating a Repository

**Questions**
  - *Where does Git store information?*

**Objectives**
  - *Create a local Git repository.*

**Keypoints**
  - *`git init` initializes a repository.*
  - *Git stores all of its repository data in the `.git` directory.*

Once Git is configured, we can start using it.

We will help Alfredo with his new project, create a repository with all his recipes.

First, let's create a directory in `Desktop` folder for our work and then move into that directory:

```shell
$ cd ~/Desktop
$ mkdir recipes
$ cd recipes
```


Then we tell Git to make `recipes` a [repository](http://swcarpentry.github.io/git-novice/reference#repository)— a place where
Git can store versions of our files:

```shell
$ git init
```


It is important to note that `git init` will create a repository that
includes subdirectories and their files---there is no need to create
separate repositories nested within the `recipes` repository, whether
subdirectories are present from the beginning or added later. Also, note
that the creation of the `recipes` directory and its initialization as a
repository are completely separate processes.

If we use `ls` to show the directory's contents,
it appears that nothing has changed:

```shell
$ ls
```


But if we add the `-a` flag to show everything,
we can see that Git has created a hidden directory within `recipes` called `.git`:

```shell
$ ls -a
.	..	.git
```

Git uses this special sub-directory to store all the information about the project, 
including all files and sub-directories located within the project's directory.
If we ever delete the `.git` sub-directory,
we will lose the project's history.

We can check that everything is set up correctly
by asking Git to tell us the status of our project:

```shell
$ git status
On branch master

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

If you are using a different version of `git`, the exact
wording of the output might be slightly different.

## Places to Create Git Repositories

Along with tracking information about recipes (the project we have already created), 
Alfredo would also like to track information about cocktails.
Despite Remy concerns, Alfredo creates a `cocktails` project inside his `recipes` 
project with the following sequence of commands:

```shell
$ cd ~/Desktop    # return to Desktop directory
$ cd recipes      # go into recipes directory, which is already a Git repository
$ ls -a           # ensure the .git sub-directory is still present in the recipes directory
$ mkdir cocktails # make a sub-directory recipes/cocktails
$ cd cocktails    # go into cocktails sub-directory
$ git init        # make the cocktails sub-directory a Git repository
$ ls -a           # ensure the .git sub-directory is present indicating we have created a new Git repository
```

Is the `git init` command, run inside the `cocktails` sub-directory, required for 
tracking files stored in the `cocktails` sub-directory?

Think about it [before checking the solution](../99_solutions/ef0e12.md).

## Correcting `git init` mistakes

Remy explains to Alfredo how a nested repository is redundant and may cause confusion
down the road. Alfredo would like to remove the nested repository. How can Alfredo undo 
his last `git init` in the `cocktails` sub-directory?

Think about it [before checking the solution](../99_solutions/70dea2.md).
