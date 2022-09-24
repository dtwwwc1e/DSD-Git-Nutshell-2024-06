# Remotes in GitLab

**Questions**
- *How do I share my changes with others on the web?*

**Objectives**
- *Explain what remote repositories are and why they are useful.*
- *Push to or pull from a remote repository.*

**Keypoints**
- *A local Git repository can be connected to one or more remote repositories.*
- *Use the HTTPS protocol to connect to remote repositories until you have learned how to set up SSH.*
- *`git push` copies changes from a local repository to a remote repository.*
- *`git pull` copies changes from a remote repository to a local repository.*

---

Version control really comes into its own when we begin to collaborate with
other people.  We already have most of the machinery we need to do this; the
only thing missing is to copy changes from one repository to another.

Systems like Git allow us to move work between any two repositories.  In
practice, though, it's easiest to use one copy as a central hub, and to keep it
on the web rather than on someone's laptop.  Most programmers use hosting
services like [GitHub](https://github.com), [BitBucket](https://bitbucket.org) or
[GitLab](https://gitlab.com/) to hold those master copies.

Let's start by sharing the changes we've made to our current project with the
rest of ISD.  Log in to [our GitLab instance](https://git.automation.ucl.ac.uk/), then click on the icon in the top right corner to
create a new project (repository) called `recipes`:

![Creating a Repository on GitLab (Step 1)](../99_images/07_gitlab_step1.png)

Name your repository "recipes" and then click "Create Repository":

![Creating a Repository on GitLab (Step 2)](../99_images/07_gitlab_step2.png)

As soon as the repository is created, GitLab displays a page with a URL and some
information on how to configure your local repository:

![Creating a Repository on GitLab (Step 3)](../99_images/07_gitlab_step3.png)

This effectively does the following on GitHub's servers:

```shell
$ mkdir recipes
$ cd recipes
$ git init
```

If you remember back to the earlier [lesson](./04-changes/README.md) where we added and
commited our earlier work on `guacamole.md`, we had a diagram of the local repository
which looked like this:

![The Local Repository with Git Staging Area](http://swcarpentry.github.io/git-novice/fig/git-staging-area.svg)

Now that we have two repositories, we need a diagram like this:

![Freshly-Made GitHub Repository](http://swcarpentry.github.io/git-novice/fig/git-freshly-made-github-repo.svg)

Note that our local repository still contains our earlier work on `guacamole.md`, but the
remote repository on GitLab appears empty as it doesn't contain any files yet.

The next step is to connect the two repositories.  We do this by making the
GitLab repository a [remote](http://swcarpentry.github.io/git-novice//reference#remote) for the local repository.
The home page of the repository on GitLab includes the string we need to
identify it:

![Where to Find Repository URL on GitLab (Step 5)](../99_images/07_gitlab_step4.png)


Copy that URL from the browser, go into the local `recipes` repository, and run
this command:

```shell
$ git remote add origin git@git.automation.ucl.ac.uk:ccealing/recipes.git
```

Make sure to use the URL for your repository rather than Alfredo's: the only
difference should be your username instead of `ccealing`.

`origin` is a local name used to refer to the remote repository. It could be called
anything, but `origin` is a convention that is often used by default in git
and GitLab, so it's helpful to stick with this unless there's a reason not to.

We can check that the command has worked by running `git remote -v`:

```shell
$ git remote -v
origin   git@git.automation.ucl.ac.uk:ccealing/recipes.git (push)
origin   git@git.automation.ucl.ac.uk:ccealing/recipes.git (fetch)
```

We'll discuss remotes in more detail in the next episode, while
talking about how they might be used for collaboration.

Once the remote is set up, this command will push the changes from
our local repository to the repository on GitHub:

```shell
$ git push origin master
Counting objects: 9, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (6/6), done.
Writing objects: 100% (9/9), 821 bytes, done.
Total 9 (delta 2), reused 0 (delta 0)
To git.automation.ucl.ac.uk:ccealing/recipes.git 
 * [new branch]      master -> master
Branch master set up to track remote branch master from origin.
```

Our local and remote repositories are now in this state:

![GitHub Repository After First Push](http://swcarpentry.github.io/git-novice/fig/github-repo-after-first-push.svg)


## Proxy

If the network you are connected to uses a proxy, there is a chance that your
last command failed with "Could not resolve hostname" as the error message. To
solve this issue, you need to tell Git about the proxy:

```
$ git config --global http.proxy http://user:password@proxy.url
$ git config --global https.proxy http://user:password@proxy.url
```

When you connect to another network that doesn't use a proxy, you will need to
tell Git to disable the proxy using:

```
$ git config --global --unset http.proxy
$ git config --global --unset https.proxy
```



## Password Managers

If you are using HTTPS instead, you will need to type your username and password
everytime you do a push.
If your operating system has a password manager configured, `git push` will
try to use it when it needs your username and password.  For example, this
is the default behavior for Git Bash on Windows. If you want to type your
username and password at the terminal instead of using a password manager,
type:

```
$ unset SSH_ASKPASS
```

in the terminal, before you run `git push`.  Despite the name, [Git uses
`SSH_ASKPASS` for all credential
entry](https://git-scm.com/docs/gitcredentials#_requesting_credentials), so
you may want to unset `SSH_ASKPASS` whether you are using Git via SSH or
https.

You may also want to add `unset SSH_ASKPASS` at the end of your `~/.bashrc`
to make Git default to using the terminal for usernames and passwords.


## The '-u' Flag

You may see a `-u` option used with `git push` in some documentation.  This
option is synonymous with the `--set-upstream-to` option for the `git branch`
command, and is used to associate the current branch with a remote branch so
that the `git pull` command can be used without any arguments. To do this,
simply use `git push -u origin master` once the remote has been set up.


We can pull changes from the remote repository to the local one as well:

```shell
$ git pull origin master
From git.automation.ucl.ac.uk:ccealing/recipes.git
 * branch            master     -> FETCH_HEAD
Already up-to-date.
```

Pulling has no effect in this case because the two repositories are already
synchronized.  If someone else had pushed some changes to the repository on
GitHub, though, this command would download them to our local repository.

