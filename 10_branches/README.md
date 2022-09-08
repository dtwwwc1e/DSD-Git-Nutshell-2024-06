# Branches

**Questions**
- *What are branches and why would I use them?*

**Objectives**
- *Create branches to separate the development of independent changes.*
- *Navigate between multiple branches*

**Keypoints**
- *Branches allow to have different parallel versions*

---

Branches on git can be understood like a machine that lets you travel
between parallel universes. Where you can check what would have happened 
if certain things would have been done differently. However, they are mostly
used to encapsulated changes related to a particular issue or feature and
they will play an important role for the merge requests that we will see later.

When a new repository is created git names the current branch as `master`, and
you've seen that every time you've run `git status`:

```shell
$ git status
On branch master
nothing to commit, working tree clean
```

But you can also see it when running:

```shell
$ git branch
* master
```

Let's create a new branch - `weird_guaca` - where we are going to experiment
with the guacamole recipe:

```shell
$ git branch weird_guaca
```

and we move ourselves to that branch as:

```shell
$ git checkout weird_guaca
```

The branch creation and travel can be done with this single command:

```shell
$ git checkout -b weird_guaca
```

At the moment, both `master` and `weird_guaca` are no different. Let's start
to modify `guacamole.md` with some fancy ingredients:

```markdown
# Ingredients
- avocado
- basil
- vinegar
- sugar
# Instructions
- peel the avocados and put them into a bowl.
```

`git status` will show now that you have modifications to add and that you are in the new branch

```shell
$ git status
On branch weird_guaca
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   guacamole.md

no changes added to commit (use "git add" and/or "git commit -a")
```

As usual, use `add` and `commit` to include this changes on the repository.

```shell
$ git add guacamole.md 
$ git commit -m 'starting to experiment'
[weird_guaca b466c98] starting to experiment
 1 file changed, 3 insertions(+), 2 deletions(-)
```

At the moment we can jump back to `master`, however that would take us to the 
same place in history as if we go to `HEAD~1` because `master` hasn't progress yet.
Let's then go to `master` and evolve in a different way.

```shell
git checkout master
```

We've seen [before]() what `checkout` does for a file or history event. In this case
we are using a branch name.

The guacamole recipe is shown now as it was previously. Let's add more ingredients:

```markdown
# Ingredients
- avocado
- plum tomatoes
- onion
- lime
- salt
- garlic
# Instructions
- peel the avocados and put them into a bowl.
```

If we commit these new changes we will have a bifurcation on our history into
two parallel universes!

```shell
$ git add guacamole.md
$ git commit -m "add important ingredients for a classical guacamole"
```

Now when you check the log you only see the history of your current branch:

```shell
$ git log
Author: Alfredo Linguini <a.linguini@ratatouille.fr> 
Date:   Thu Aug 22 13:14:20 2019 -0400

    add important ingredients for a classical guacamole

commit 049e2710823ff1535fde3225be569185be4732b6
Author: Alfredo Linguini <a.linguini@ratatouille.fr>
Date:   Thu Aug 22 10:55:21 2019 -0400

    Merge changes from GitLab
```

But you could see a graph of where the changes are using `--all` and `--graph`

```shell
$ git log --all --decorate --oneline --graph
* d304550 (HEAD -> master) add important ingredients for a classical guacamole
| * b466c98 (weird_guaca) starting to experiment
|/
* 049e271 Merge changes from GitLab
* 005937f Modify guacamole to the traditional recipe
* 34961b1 Add basic guacamole's ingredients
* f22b25e Create a template for recipe
```

These two branches can live as much as you want, but it's good practice to
bring them together at some time. The can combined using `git merge`, so if we
want to bring the `weird_guaca` branch on master we can do - from `master`:

```shell
$ git merge weird_guaca
Auto-merging guacamole.md
CONFLICT (content): Merge conflict in guacamole.md
Automatic merge failed; fix conflicts and then commit the result.
```

In this case you will find that there are conflicts as both branches touched the
same lines. Fix the conflict, save and commit. If you don't add a comment then
git will open your text editor with an automated message `Merge branch
'weird_guaca'`. Now the logs show the following

```shell
$ git log --all --decorate --oneline --graph
*   f694ccb (HEAD -> master) Merge branch 'weird_guaca'
|\ 
| * b466c98 (weird_guaca) starting to experiment
* | d304550 add important ingredients for a classical guacamole
|/
* 049e271 Merge changes from GitLab
* 005937f Modify guacamole to the traditional recipe
* 34961b1 Add basic guacamole's ingredients
* f22b25e Create a template for recipe
```

## Listing all the branches

To see all the branches you have in your local repository you only need to add `--list` to the branch command:

```shell
$ git branch --list
* master
  weird_guaca
```

The one marked with a `*` is the one you are in at the moment. But if you need also to know what's the last that 
have happened in that branch, then `-v` is more useful:

```shell
$ git branch -v
* master      f694ccb Merge branch 'weird_guaca'
  weird_guaca b466c98 starting to experiment
```

## Publishing branches

Git allows you to only synchronise the branches that you want between the
remotes. This is, that you can have multiple branches locally but only some of them on
GitLab. In other words, you need to tell git what you want to send and to where.

As we done before, this is done via `push` and `fetch`.

```shell
git push -u origin weird_guaca
```

will send the branch to your origin. We use `--set-upstream` origin (Abbreviation
`-u`) to tell git that this branch should be pushed to and pulled from origin per
default.

When we do `git fetch` we fetch these branches, but we don't create them locally. Change to the directory where
you cloned your partner's repository and do:

```shell
$ cd ../ccealing_recipes
$ git fetch
remote: Enumerating objects: 22, done.
remote: Counting objects: 100% (22/22), done.
remote: Compressing objects: 100% (11/11), done.
remote: Total 22 (delta 12), reused 21 (delta 11), pack-reused 0
Unpacking objects: 100% (22/22), done.
From git.automation.ucl.ac.uk:ccealing/recipes.git
 * [new branch]      weird_guaca -> origin/weird_guaca
```

To change to that branch you will need then to create it locally as:

```shell
$ git checkout -b weird_guaca origin/weird_guaca
Branch weird_guaca' set up to track remote branch 'weird_guaca from 'origin'.
Switched to a new branch 'weird_guaca''
```

## Cleaning up after a branch

It's good practice to keep your repositories clean of outdated or merged branches.
Deleting branches is possible with:

```shell
$ git branch -d weird_guaca
```

However, that only deletes it on your local machine, not in other remote repositories.

You can see that it still exists on gitlab:

```shell
$ git branch --remote
  origin/master
  origin/weird_guaca
```

To delete there too you can either do it through the web interface or:

```shell
$ git push --delete origin weird_guaca
To git.automation.ucl.ac.uk:ccealing/recipes.git

 [deleted] weird_guaca
```

## A good branch strategy

Each team or project must have a defined branch strategy, in these it will
define the accepted practices regarding the purpose of the main branches and the
naming convention to use. For example, the [DevOps group has defined a
workflow](https://wiki.ucl.ac.uk/pages/viewpage.action?spaceKey=DP&title=GitLab+User+Workflow).

In summary, you should have something like:

- A `production` branch: code used for active work or stable releases;
- A `develop` branch: for general new code;
- `feature` branches: for specific new ideas; and a 
- `release` branches: when you share code with others. Useful for isolated bug fixes
