# Conflicts

**Questions**
- *What do I do when my changes conflict with someone else's?*

**Objectives**
- *Explain what conflicts are and when they can occur.*
- *Resolve conflicts resulting from a merge.*

**Keypoints**
- *Conflicts occur when two or more people change the same file(s) at the same time.*
- *The version control system does not allow people to overwrite each other's changes blindly, but highlights conflicts so that they can be resolved.*

---

As soon as people can work in parallel, they'll likely step on each other's
toes.  This will even happen with a single person: if we are working on
a piece of software on both our laptop and a server in the lab, we could make
different changes to each copy.  Version control helps us manage these
[conflicts](http://swcarpentry.github.io/git-novice//reference#conflicts) by giving us tools to
[resolve](http://swcarpentry.github.io/git-novice//reference#resolve) overlapping changes.

To see how we can resolve conflicts, we must first create one.  The file
`guacamole.md` currently looks like this in both partners' copies of our `recipes`
repository:

```markdown
# Ingredients
- avocado
- lime
- salt
# Instructions
```

Let's add a line to one partner's copy only:

```markdown
# Ingredients
- avocado
- lime
- salt
# Instructions
- put one avocado into a bowl.
```

and then push the change to GitLab:

```shell
$ git add guacamole.md
$ git commit -m "First step on the instructions"
[master 5ae9631] First step on the instructions
 1 file changed, 1 insertion(+)
$ git push origin master
Counting objects: 5, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 352 bytes, done.
Total 3 (delta 1), reused 0 (delta 0)
To git@git.automation.ucl.ac.uk:ccealing/recipes.git
   29aba7c..dabb4c8  master -> master
```

Now let's have the other partner
make a different change to their copy
*without* updating from GitHub:

```shell
$ nano guacamole.md
```

```markdown
# Ingredients
- avocado
- lime
- salt
# Instructions
- peel the avocados
```

We can commit the change locally:

```shell
$ git add guacamole.md
$ git commit -m "Add first step"
[master 07ebc69] Add first step
 1 file changed, 1 insertion(+)
```

but Git won't let us push it to GitLab:

```shell
$ git push origin master
To git.automation.ucl.ac.uk:ccealing/recipes.git
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'git.automation.ucl.ac.uk:ccealing/recipes.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Merge the remote changes (e.g. 'git pull')
hint: before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

![The Conflicting Changes](http://swcarpentry.github.io/git-novice/fig/conflict.svg)

Git rejects the push because it detects that the remote repository has new updates that have not been
incorporated into the local branch.
What we have to do is pull the changes from GitLab,
[merge](http://swcarpentry.github.io/git-novice//reference#merge) them into the copy we're currently working in,
and then push that.
Let's start by pulling:

```shell
$ git pull origin master
remote: Counting objects: 5, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 1), reused 3 (delta 1)
Unpacking objects: 100% (3/3), done.
From git.automation.ucl.ac.uk:ccealing/recipes.git
 * branch            master     -> FETCH_HEAD
Auto-merging guacamole.md
CONFLICT (content): Merge conflict in guacamole.md
Automatic merge failed; fix conflicts and then commit the result.
```

The `git pull` command updates the local repository to include those
changes already included in the remote repository.
After the changes from remote branch have been fetched, Git detects that changes made to the local copy 
overlap with those made to the remote repository, and therefore refuses to merge the two versions to
stop us from trampling on our previous work. The conflict is marked in
in the affected file:


```markdown
# Ingredients
- avocado
- lime
- salt
# Instructions
<<<<<<< HEAD
- peel the avocados
=======
- put one avocado into a bowl.
>>>>>>> dabb4c8c450e8475aee9b14b4383acc99f42af1d
```

Our change is preceded by `<<<<<<< HEAD`.
Git has then inserted `=======` as a separator between the conflicting changes
and marked the end of the content downloaded from GitLab with `>>>>>>>`.
(The string of letters and digits after that marker
identifies the commit we've just downloaded.)

It is now up to us to edit this file to remove these markers
and reconcile the changes.
We can do anything we want: keep the change made in the local repository, keep
the change made in the remote repository, write something new to replace both,
or get rid of the change entirely.
Let's replace both so that the file looks like this:


```markdown
# Ingredients
- avocado
- lime
- salt
# Instructions
- peel the avocados and put them into a bowl.
```

To finish merging,
we add `guacamole.md` to the changes being made by the merge
and then commit:

```shell
$ git add guacamole.md
$ git status
On branch master
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)

Changes to be committed:

	modified:   guacamole.md

$ git commit -m "Merge changes from GitLab"
[master 2abf2b1] Merge changes from GitLab
```

Now we can push our changes to GitLab:

```shell
$ git push origin master
Counting objects: 10, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (6/6), done.
Writing objects: 100% (6/6), 697 bytes, done.
Total 6 (delta 2), reused 0 (delta 0)
To git.automation.ucl.ac.uk:ccealing/recipes
   dabb4c8..2abf2b1  master -> master
```

Git keeps track of what we've merged with what,
so we don't have to fix things by hand again
when the collaborator who made the first change pulls again:

```shell
$ git pull origin master
remote: Counting objects: 10, done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 6 (delta 2), reused 6 (delta 2)
Unpacking objects: 100% (6/6), done.
From git.automation.ucl.ac.uk:ccealing/recipes
 * branch            master     -> FETCH_HEAD
Updating dabb4c8..2abf2b1
Fast-forward
 guacamole.md | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
```

We get the merged file:

```markdown
# Ingredients
- avocado
- lime
- salt
# Instructions
- peel the avocados and put them into a bowl.
```

We don't need to merge again because Git knows someone has already done that.

Git's ability to resolve conflicts is very useful, but conflict resolution
costs time and effort, and can introduce errors if conflicts are not resolved
correctly. If you find yourself resolving a lot of conflicts in a project,
consider these technical approaches to reducing them:

- Pull from upstream more frequently, especially before starting new work
- Use topic branches to segregate work, merging to master when complete
- Make smaller more atomic commits
- Where logically appropriate, break large files into smaller ones so that it is
  less likely that two authors will alter the same file simultaneously

Conflicts can also be minimized with project management strategies:

- Clarify who is responsible for what areas with your collaborators
- Discuss what order tasks should be carried out in with your collaborators so
  that tasks expected to change the same lines won't be worked on simultaneously
- If the conflicts are stylistic churn (e.g. tabs vs. spaces), establish a
  project convention that is governing and use code style tools (e.g.
  `htmltidy`, `perltidy`, `rubocop`, etc.) to enforce, if necessary

