# Tracking Changes

**Questions**
- *How do I record changes in Git?*
- *How do I check the status of my version control repository?*
- *How do I record notes about what changes I made and why?*

**Objectives**
- *Go through the modify-add-commit cycle for one or more files.*
- *Explain where information is stored at each stage of that cycle.*
- *Distinguish between descriptive and non-descriptive commit messages.*

**Keypoints**
- *`git status` shows the status of a repository.*
- *Files can be stored in a project's working directory (which users see), the staging area (where the next commit is being built up) and the local repository (where commits are permanently recorded).*
- *`git add` puts files in the staging area.*
- *`git commit` saves the staged content as a new commit in the local repository.*
- *Write a commit message that accurately describes your changes.*

First let's make sure we're still in the right directory.
You should be in the `recipes` directory.

```shell
$ pwd
/home/chef/Desktop/recipes
```

If you are still in `cocktails`, navigate back up to `recipes`

```shell
$ pwd
/home/chef/Desktop/recipes/cocktails
$ cd ..
```


Let's create a file called `guacamole.md` that contains the basic structure to
have a recipe. We'll use `nano` to edit the file; you can use whatever editor
you like. In particular, this does not have to be the `core.editor` you set
globally earlier. But remember, the bash command to create or edit a new file
will depend on the editor you choose (it might not be `nano`). For a refresher
on text editors, check out ["Which
Editor?"](https://swcarpentry.github.io/shell-novice/03-create/) in [The Unix
Shell](https://swcarpentry.github.io/shell-novice/) lesson.

```shell
$ nano guacamole.md
```

Type the text below into the `guacamole.md` file:

```markdown
# Ingredients
# Instructions
```

`guacamole.md` now contains two lines, which we can see by running:

```shell
$ ls
guacamole.md
$ cat guacamole.md
# Ingredients
# Instructions
```

If we check the status of our project again,
Git tells us that it's noticed the new file:

```shell
$ git status
On branch master

Initial commit

Untracked files:
   (use "git add <file>..." to include in what will be committed)

	guacamole.md
nothing added to commit but untracked files present (use "git add" to track)
```

The "untracked files" message means that there's a file in the directory
that Git isn't keeping track of.
We can tell Git to track a file using `git add`:

```shell
$ git add guacamole.md
```

and then check that the right thing happened:

```shell
$ git status
On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   guacamole.md

```

Git now knows that it's supposed to keep track of `guacamole.md`,
but it hasn't recorded these changes as a commit yet.
To get it to do that,
we need to run one more command:

```shell
$ git commit -m "Create a template for recipe"
[master (root-commit) f22b25e] Create a template for recipe
 1 file changed, 2 insertion(+)
 create mode 100644 guacamole.md
```

When we run `git commit`,
Git takes everything we have told it to save by using `git add`
and stores a copy permanently inside the special `.git` directory.
This permanent copy is called a [commit](http://swcarpentry.github.io/git-novice//reference#commit)
(or [revision](http://swcarpentry.github.io/git-novice//reference#revision)) and its short identifier is `f22b25e`.
Your commit may have another identifier.

We use the `-m` flag (for "message")
to record a short, descriptive, and specific comment that will help us remember later on what we did and why.
If we just run `git commit` without the `-m` option,
Git will launch `nano` (or whatever other editor we configured as `core.editor`)
so that we can write a longer message.

[Good commit messages][commit-messages] start with a brief (<50 characters) statement about the
changes made in the commit. Generally, the message should complete the sentence "If applied, this commit will" <commit message here>.
If you want to go into more detail, add a blank line between the summary line and your additional notes. Use this additional space to explain why you made changes and/or what their impact will be.

If we run `git status` now:

```shell
$ git status
On branch master
nothing to commit, working directory clean
```

it tells us everything is up to date.
If we want to know what we've done recently,
we can ask Git to show us the project's history using `git log`:

```shell
$ git log
commit f22b25e3233b4645dabd0d81e651fe074bd8e73b
Author: Alfredo Linguini <a.linguini@ratatouille.fr>
Date:   Thu Aug 22 09:51:46 2013 -0400

    Create a template for recipe
```

`git log` lists all commits  made to a repository in reverse chronological order.
The listing for each commit includes
the commit's full identifier
(which starts with the same characters as
the short identifier printed by the `git commit` command earlier),
the commit's author,
when it was created,
and the log message Git was given when the commit was created.

## Where Are My Changes?

If we run `ls` at this point, we will still see just one file called `guacamole.md`.
That's because Git saves information about files' history
in the special `.git` directory mentioned earlier
so that our filesystem doesn't become cluttered
(and so that we can't accidentally edit or delete an old version).

Now suppose Alfredo adds more information to the file.
(Again, we'll edit with `nano` and then `cat` the file to show its contents;
you may use a different editor, and don't need to `cat`.)

```shell
$ nano guacamole.md
```
```markdown
# Ingredients
- avocado
- lemon
- salt
# Instructions
```

When we run `git status` now,
it tells us that a file it already knows about has been modified:

```shell
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   guacamole.md

no changes added to commit (use "git add" and/or "git commit -a")
```

The last line is the key phrase:
"no changes added to commit".
We have changed this file,
but we haven't told Git we will want to save those changes
(which we do with `git add`)
nor have we saved them (which we do with `git commit`).
So let's do that now. It is good practice to always review
our changes before saving them. We do this using `git diff`.
This shows us the differences between the current state
of the file and the most recently saved version:

```shell
$ git diff
diff --git a/guacamole.md b/guacamole.md
index df0654a..315bf3a 100644
--- a/guacamole.md
+++ b/guacamole.md
@@ -1,2 +1,5 @@
 # Ingredients
+- avocado
+- lemon
+- salt
 # Instructions
```

The output is cryptic because
it is actually a series of commands for tools like editors and `patch`
telling them how to reconstruct one file given the other.
If we break it down into pieces:

1.  The first line tells us that Git is producing output similar to the Unix `diff` command
    comparing the old and new versions of the file.
2.  The second line tells exactly which versions of the file
    Git is comparing;
    `df0654a` and `315bf3a` are unique computer-generated labels for those versions.
3.  The third and fourth lines once again show the name of the file being changed.
4.  The remaining lines are the most interesting, they show us the actual differences
    and the lines on which they occur.
    In particular,
    the `+` marker in the first column shows where we added a line.

After reviewing our change, it's time to commit it:

```shell
$ git commit -m "Add basic guacamole's ingredients"
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   guacamole.md

no changes added to commit (use "git add" and/or "git commit -a")
```

Whoops:
Git won't commit because we didn't use `git add` first.
Let's fix that:

```shell
$ git add guacamole.md
$ git commit -m "Add basic guacamole's ingredients"
[master 34961b1] Add basic guacamole's ingredients"
 1 file changed, 1 insertion(+)
```

Git insists that we add files to the set we want to commit
before actually committing anything. This allows us to commit our
changes in stages and capture changes in logical portions rather than
only large batches.
For example,
suppose we're adding a list with dependencies to our code.
We might want to commit those additions,
and the corresponding lines where these are used,
but *not* commit some of our other work drafting some new implementation
(which we haven't finished yet).

To allow for this,
Git has a special *staging area*
where it keeps track of things that have been added to
the current [changeset](http://swcarpentry.github.io/git-novice//reference#changeset)
but not yet committed.

## Staging Area

If you think of Git as taking snapshots of changes over the life of a project,
`git add` specifies *what* will go in a snapshot
(putting things in the staging area),
and `git commit` then *actually takes* the snapshot, and
makes a permanent record of it (as a commit).
If you don't have anything staged when you type `git commit`,
Git will prompt you to use `git commit -a` or `git commit --all`,
which is kind of like gathering *everyone* for the picture!
However, it's almost always better to
explicitly add things to the staging area, because you might
commit changes you forgot you made. (Going back to snapshots,
you might get the extra with incomplete makeup walking on
the stage for the snapshot because you used `-a`!)
Try to stage things manually,
or you might find yourself searching for "git undo commit" more
than you would like!


![The Git Staging Area](http://swcarpentry.github.io/git-novice/fig/git-staging-area.svg)

Let's watch as our changes to a file move from our editor
to the staging area
and into long-term storage.
First,
we'll add another line to the file:

```shell
$ nano guacamole.md
```

```markdown
# Ingredients
- avocado
- lime
- salt
# Instructions
```

```shell
$ git diff
diff --git a/guacamole.md b/guacamole.md
index 315bf3a..b36abfd 100644
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

So far, so good:
we've modified one line
(shown with a `+` in the first column).
Now let's put that change in the staging area
and see what `git diff` reports:

```shell
$ git add guacamole.md
$ git diff
```

There is no output:
as far as Git can tell,
there's no difference between what it's been asked to save permanently
and what's currently in the directory.
However,
if we do this:

```shell
$ git diff --staged
diff --git a/guacamole.md b/guacamole.md
index 315bf3a..b36abfd 100644
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

it shows us the difference between
the last committed change
and what's in the staging area.
Let's save our changes:

```shell
$ git commit -m "Modify guacamole to the traditional recipe"
[master 005937f] Modify guacamole to the traditional recipe
 1 file changed, 1 insertion(+)
```

check our status:

```shell
$ git status
On branch master
nothing to commit, working directory clean
```

and look at the history of what we've done so far:

```shell
$ git log
commit 005937fbe2a98fb83f0ade869025dc2636b4dad5
Author: Alfredo Linguini <a.linguini@ratatouille.fr>
Date:   Thu Aug 22 10:14:07 2013 -0400

    Modify guacamole to the traditional recipe

commit 34961b159c27df3b475cfe4415d94a6d1fcd064d
Author: Alfredo Linguini <a.linguini@ratatouille.fr>
Date:   Thu Aug 22 10:07:21 2013 -0400

    Add basic guacamole's ingredients

commit f22b25e3233b4645dabd0d81e651fe074bd8e73b
Author: Alfredo Linguini <a.linguini@ratatouille.fr>
Date:   Thu Aug 22 09:51:46 2013 -0400

    Create a template for recipe
```


## Word-based diffing

Sometimes, e.g. in the case of the text documents a line-wise
diff is too coarse. That is where the `--color-words` option of
`git diff` comes in very useful as it highlights the changed
words using colors.


## Paging the Log

When the output of `git log` is too long to fit in your screen,
`git` uses a program to split it into pages of the size of your screen.
When this "pager" is called, you will notice that the last line in your
screen is a `:`, instead of your usual prompt.
*   To get out of the pager, press <kbd>q</kbd>.
*   To move to the next page, press <kbd>Spacebar</kbd>.
*   To search for `some_word` in all pages,
    press <kbd>/</kbd>
    and type `some_word`.
    Navigate through matches pressing <kbd>n</kbd>/<kbd>N</kbd> (next/previous).


## Limit Log Size

To avoid having `git log` cover your entire terminal screen, you can limit the
number of commits that Git lists by using `-N`, where `N` is the number of
commits that you want to view. For example, if you only want information from
the last commit you can use:

```shell
$ git log -1
commit 005937fbe2a98fb83f0ade869025dc2636b4dad5
Author: Alfredo Linguini <a.linguini@ratatouille.fr>
Date:   Thu Aug 22 10:14:07 2013 -0400

   Modify guacamole to the traditional recipe
```

You can also reduce the quantity of information using the
`--oneline` option:

```shell
$ git log --oneline
* 005937f Modify guacamole to the traditional recipe
* 34961b1 Add basic guacamole's ingredients
* f22b25e Create a template for recipe
```

You can also combine the `--oneline` options with others. One useful
combination is:

```shell
$ git log --oneline --graph --all --decorate
* 005937f Modify guacamole to the traditional recipe  (HEAD, master)
* 34961b1 Add basic guacamole's ingredients
* f22b25e Create a template for recipe
```

## Directories

Two important facts you should know about directories in Git.

1. Git does not track directories on their own, only files within them.
   Try it for yourself:
   ```shell
   $ mkdir cakes
   $ git status
   $ git add cakes
   $ git status
   ```

   Note, our newly created empty directory `cakes` does not appear in
   the list of untracked files even if we explicitly add it (_via_ `git add`) to our
   repository. This is the reason why you will sometimes see `.gitkeep` files
   in otherwise empty directories. Unlike `.gitignore`, these files are not special
   and their sole purpose is to populate a directory so that Git adds it to
   the repository. In fact, you can name such files anything you like.

2. If you create a directory in your Git repository and populate it with files,
   you can add all files in the directory at once by:

   ```shell
   git add <directory-with-files>
   ```
   
   Try it for yourself:

   ```shell
   $ touch cakes/brownie cakes/lemon_drizzle
   $ git status
   $ git add cakes
   $ git status
   ```
   
   Before moving on, we will commit these changes.

   ```shell
   $ git commit -m "Add some initial cakes"
   ```
   
To recap, when we want to add changes to our repository,
we first need to add the changed files to the staging area
(`git add`) and then commit the staged changes to the
repository (`git commit`):

![The Git Commit Workflow](http://swcarpentry.github.io/git-novice/fig/git-committing.svg)


[commit-messages]: https://chris.beams.io/posts/git-commit/
