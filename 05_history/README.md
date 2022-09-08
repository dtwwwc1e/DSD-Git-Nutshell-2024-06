# Exploring History

**Questions**
- *How can I identify old versions of files?*
- *How do I review my changes?*
- *How can I recover old versions of files?*

**Objectives**
- *Explain what the HEAD of a repository is and how to use it.*
- *Identify and use Git commit numbers.*
- *Compare various versions of tracked files.*
- *Restore old versions of files.*

**Keypoints**
- *`git diff` displays differences between commits.*
- *`git checkout` recovers old versions of files.*

---

As we saw in the previous lesson, we can refer to commits by their
identifiers.  You can refer to the _most recent commit_ of the working
directory by using the identifier `HEAD`.

We've been adding small changes at a time to `guacamole.md`, so it's easy to track our
progress by looking, so let's do that using our `HEAD`s.

```shell
$ git diff HEAD~1 guacamole.md
diff --git a/guacamole.md b/guacamole.md
index b36abfd..0848c8d 100644
--- a/guacamole.md
+++ b/guacamole.md
@@ -1,5 +1,5 @@
 # Ingredients
 - avocado
-- lemon
+- lime
 - salt
 # Instructions
```

With `~1` appended to `HEAD` we are referring to previous commits.
(where "~" is "tilde", pronounced [**til**-d*uh*]) 
to refer to the commit one before `HEAD`.

If we want to see the differences between older commits we can use `git diff`
again, but with the notation `HEAD~1`, `HEAD~2`, and so on, to refer to them:

```shell
$ git diff HEAD~2 guacamole.md
diff --git a/guacamole.md b/guacamole.md
index df0654a..b36abfd 100644
--- a/guacamole.md
+++ b/guacamole.md
@@ -1,2 +1,5 @@
 # Ingredients
+- avocado
+- lime
+- salt
 # Instructions
```

We could also use `git show` which shows us what changes we made at an older
commit as well as the commit message, rather than the _differences_ between a
commit and our working directory that we see by using `git diff`.

```shell
$ git show HEAD~2 guacamole.md
commit 34961b159c27df3b475cfe4415d94a6d1fcd064d
Author: Alfredo Linguini <a.linguini@ratatouille.fr>
Date:   Thu Aug 22 10:07:21 2013 -0400

    Create a template for recipe

diff --git a/guacamole.md b/guacamole.md
new file mode 100644
index 0000000..df0654a
--- /dev/null
+++ b/guacamole.md
@@ -0,0 +1,2 @@
+# Ingredients
+# Instructions
```

In this way,
we can build up a chain of commits.
The most recent end of the chain is referred to as `HEAD`;
we can refer to previous commits using the `~` notation,
so `HEAD~1`
means "the previous commit",
while `HEAD~123` goes back 123 commits from where we are now.

We can also refer to commits using
those long strings of digits and letters
that `git log` displays.
These are unique IDs for the changes,
and "unique" really does mean unique:
every change to any set of files on any computer
has a unique 40-character identifier.
Our first commit was given the ID
`f22b25e3233b4645dabd0d81e651fe074bd8e73b`,
so let's try this:

```shell
$ git diff f22b25e3233b4645dabd0d81e651fe074bd8e73b guacamole.md
diff --git a/guacamole.md b/guacamole.md
index df0654a..93a3e13 100644
--- a/guacamole.md
+++ b/guacamole.md
@@ -1,2 +1,5 @@
 # Ingredients
+- avocado
+- lime
+- salt
 # Instructions
```

That's the right answer,
but typing out random 40-character strings is annoying,
so Git lets us use just the first few characters:

```
$ git diff f22b25e guacamole.md
diff --git a/guacamole.md b/guacamole.md
index df0654a..93a3e13 100644
--- a/guacamole.md
+++ b/guacamole.md
@@ -1,2 +1,5 @@
 # Ingredients
+- avocado
+- lime
+- salt
 # Instructions
```

All right! So
we can save changes to files and see what we've changed—now how
can we restore older versions of things?

Let's suppose we overwrite our file by mistake, like deleting all the content of
`guacamole.md` and saving the changes (the "ill-considered change").

`git status` now tells us that the file has been changed,
but those changes haven't been staged:

```shell
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   guacamole.md

no changes added to commit (use "git add" and/or "git commit -a")
```

We can put things back the way they were
by using `git checkout`:

```shell
$ git checkout HEAD guacamole.md
$ cat guacamole.md
- avocado
- lime
- salt
# Instructions
```

As you might guess from its name,
`git checkout` checks out (i.e., restores) an old version of a file.
In this case,
we're telling Git that we want to recover the version of the file recorded in `HEAD`,
which is the last saved commit.
If we want to go back even further,
we can use a commit identifier instead:

```shell
$ git checkout f22b25e guacamole.md
$ cat guacamole.md
# Ingredients
# Instructions
```

```shell
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   guacamole.md

```

Notice that the changes are on the staged area.
Again, we can put things back the way they were
by using `git checkout`:

```shell
$ git checkout HEAD guacamole.md
```


## Don't Lose Your HEAD

Above we used

```shell
$ git checkout f22b25e guacamole.md
```

to revert `guacamole.md` to its state after the commit `f22b25e`. But be careful! 
The command `checkout` has other important functionalities and Git will misunderstand
your intentions if you are not accurate with the typing. For example, 
if you forget `guacamole.md` in the previous command.

```shell
$ git checkout f22b25e
Note: checking out 'f22b25e'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -b with the checkout command again. Example:

 git checkout -b <new-branch-name>

HEAD is now at f22b25e Start notes on Mars as a base
```

The "detached HEAD" is like "look, but don't touch" here,
so you shouldn't make any changes in this state.
After investigating your repo's past state, reattach your `HEAD` with `git checkout master`.


It's important to remember that
we must use the commit number that identifies the state of the repository
*before* the change we're trying to undo.
A common mistake is to use the number of
the commit in which we made the change we're trying to get rid of.
In the example below, we want to retrieve the state from before the most
recent commit (`HEAD~1`), which is commit `f22b25e`:

![Git Checkout](http://swcarpentry.github.io/git-novice/fig/git-checkout.svg)

So, to put it all together,
here's how Git works in cartoon form:

![httpshttp://swcarpentry.github.io/git-novice/figshare.com/articles/How_Git_works_a_cartoon/1328266](http://swcarpentry.github.io/git-novice/fig/git_staging.svg)

## Simplifying the Common Case

If you read the output of `git status` carefully,
you'll see that it includes this hint:

```shell
(use "git checkout -- <file>..." to discard changes in working directory)
```

As it says,
`git checkout` without a version identifier restores files to the state saved in `HEAD`.
The double dash `--` is needed to separate the names of the files being recovered
from the command itself:
without it,
Git would try to use the name of the file as the commit identifier.


The fact that files can be reverted one by one
tends to change the way people organize their work.
If everything is in one large document,
it's hard (but not impossible) to undo changes to the introduction
without also undoing changes made later to the conclusion.
If the introduction and conclusion are stored in separate files,
on the other hand,
moving backward and forward in time becomes much easier.

