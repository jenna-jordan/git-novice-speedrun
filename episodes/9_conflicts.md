---
title: "[Post-workshop] Resolving Conflicts"
teaching: 15
exercises: 1
---

:::::::::::::::::::::::::::::::::::::: questions 

- What do I do when my changes conflict?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Explain what conflicts are and when they can occur.
- Understand how to resolve conflicts resulting from a merge.

::::::::::::::::::::::::::::::::::::::::::::::::


As soon as people can work in parallel, they'll likely step on each other's
toes.  This will even happen with a single person: if we are working on
a piece of software on both our laptop and a server in the lab, we could make
different changes to each copy.  Version control helps us manage these
[conflicts](/instructor/reference/#conflicts) by giving us tools to
[resolve](/instructor/reference/#resolve) overlapping changes.

To see how we can resolve conflicts, we must first create one.  The
file `mars.txt` currently looks like this in our `planets` repository:

```output
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity

```

Let's create a new branch for discussing Mars' temperature and
checkout that branch.

```bash
$ git branch mars-temp
```

But before we make changes related to Mars' temperature in the `mars-temp`
branch, let's add a line to mars.txt here in the `main` branch.

```output
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
I'll be able to get 40 extra minutes of beauty rest

```

and commit that change to the `main` branch

```bash
$ git add mars.txt
$ git commit -m "Add a line about the daylight on Mars."
```

```output
[main 5ae9631] Add a line about the daylight on Mars.
 1 file changed, 1 insertion(+)
```

We can then examine the commit history of the `main` branch.

```bash
$ git log --oneline
```

```output
60e140a (HEAD -main) Add a line about the daylight on Mars.
d980f78 (origin/main, mars-temp) Merge pull request #1 from vlad/pythondev
d5f2565 (origin/pythondev, pythondev) Wrote and tested python analysis script
e98a594 Discuss concerns about Mars' climate for Mummy
33d27e2 Add concerns about effects of Mars' moons on Wolfman
7e1e559 Start notes on Mars as a base
```

Now that we've made our changes in the `main` branch, let's get to work on our comments about
the temperature in the `mars-temp` branch.

```bash
$ git checkout mars-temp
$ git branch
```

```output
* mars-temp
  main
```

Let's make a note in `mars.txt` about the temperature. Note that when we open
this file the line we added about the daylight on Mars will not be present as
that change is not part of this branch.

```output
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
Yeti will appreciate the cold

```

Now let's commit this change to the `mars-temp` branch

```bash
$ git add mars.txt
$ git commit -m "Add a line about the temperature on Mars"
```

```output
[main 07ebc69] Add a line about the temperature on Mars
 1 file changed, 1 insertion(+)
```

Again, we can look at the history of this branch.

```bash
$ git log --oneline
```

```output
847c94d (HEAD -mars-temp) Add a line about the temperature on Mars
d980f78 (origin/main) Merge pull request #1 from vlad/pythondev
d5f2565 (origin/pythondev, pythondev) Wrote and tested python analysis script
e98a594 Discuss concerns about Mars' climate for Mummy
33d27e2 Add concerns about effects of Mars' moons on Wolfman
7e1e559 Start notes on Mars as a base
```

::::::::::::::: callout

Notice that the commit related to Mars' daylight is not present as it is part of
the `main` branch, not the `mars-temp` branch.

:::::::::::::::::::

Now that we've added changes about the temperature
we can merge them into the `main` branch. We're going to do this merge in VS Code rather than through a Pull Request in GitHub this time.

First, let's checkout the
`main` branch.

```bash
$ git checkout main
$ git branch
```

```output
  mars-temp
* main
```

And then merge the changes from `mars-temp` into our current branch, `main`.

```bash
$ git merge mars-temp
```

```output
Auto-merging mars.txt
CONFLICT (content): Merge conflict in mars.txt
Automatic merge failed; fix conflicts and then commit the result.
```

Review the status of the repository now that we've been told merging has resulted in a conflict.

```bash
$ git status
```

```output
On branch main
You have unmerged paths.
  (fix conflicts and run "git commit")

Unmerged paths:
  (use "git add <file>..." to mark resolution)

  both modified:   mars.txt

no changes added to commit (use "git add" and/or "git commit -a")

```

![The Conflicting Changes](fig/conflict.svg)

Git detects that the changes made in one copy overlap with those made
in the other and stops us from trampling on our previous work. It also
marks that conflict in the affected file, `mars.txt`.

```output
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
<<<<<<< HEAD
I'll be able to get 40 extra minutes of beauty rest
=======
Yeti will appreciate the cold
>>>>>>07ebc69c450e8475aee9b14b4383acc99f42af1d
```

Our change—the one at the `HEAD` of the `main` branch—is preceded by `<<<<<<<`.
Git has then inserted `=======` as a separator between the conflicting changes
and marked the end of our commit from the `mars-temp` branch with `>>>>>>>`.
(The string of letters and digits after that marker
identifies the commit we made in the `mars-temp` branch.)

It is now up to us to edit this file to remove these markers
and reconcile the changes.
We can do anything we want: keep the change made in the `main` branch, keep
the change made in the `mars-temp` branch, write something new to replace both,
or get rid of the change entirely.

Let's keep both of these statements, as they are both valid regarding the Martian
environment.

VS Code will prompt you to "Resolve in Merge Editor". Click this button.

In the Merge Editor, you will see the two versions side by side, and the final version of the file below. We want the final version to look like:

```output
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
I'll be able to get 40 extra minutes of beauty rest
Yeti will appreciate the cold

```

So we want to click "Accept Combination (Current First)". You should now see the resolved version with both lines, the line from the `main` branch first. Click "Complete Merge".

By clicking "Complete Merge", you are doing the same thing as running the command `git add mars.txt`. If you open the Source Control pane, you should see `mars.txt` under "Staged Changes". Let's also check the git status.

```bash
$ git status
```

```output
On branch main
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)

Changes to be committed:

	modified:   mars.txt

```

```bash
$ git commit -m "Merge changes from mars-temp"
```

```output
[main 2abf2b1] Merge changes from mars-temp
```

Take a look at the Source Control Graph window (in the lower portion of the Source Control pane) for a visual representation of the git log.

Git keeps track of what we've merged with what,
so we don't have to fix things by hand again.

Let's make another change to the `mars-temp` branch:

```
$ git checkout mars-temp
```

Add another line to `mars.txt`:

```output
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
Yeti will appreciate the cold
The polar caps will probably be Yeti's home

```

```bash
$ git add mars.txt
$ git commit -m "A note about Yeti's home"
```

```output
[main 34avo82] A note about Yeti's home
 1 file changed, 1 insertion(+)
```

And merge that change into main branch

```bash
$ git checkout main
$ git merge mars-temp
```

```output
Updating 12687f6..x792csa1
Fast-forward
 mars.txt | 1 +
 1 file changed, 1 insertions(+), 0 deletions(-)
```

There is no conflict and our changes are added automatically

::::::::::::::::::: callout
## Still seeing a conflict?

This exercise is dependent on how the merge conflict was resolved
in our first merge of the mars-temp branch and may still result
in a conflict when merging additional commits from the mars-temp branch.

::::::::::::::::::::::::::

```output
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
I'll be able to get 40 extra minutes of beauty rest
Yeti will appreciate the cold
The polar caps will probably be Yeti's home
```

We don't need to merge again because Git knows someone has already done that.

Git's ability to resolve conflicts is very useful, but conflict resolution
costs time and effort, and can introduce errors if conflicts are not resolved
correctly. If you find yourself resolving a lot of conflicts in a project,
consider these technical approaches to reducing them:

- Pull from upstream more frequently, especially before starting new work
- Use topic branches to separate work, merging to main when complete
- Make smaller more atomic commits
- Where logically appropriate, break large files into smaller ones so that it is
  less likely that two authors will alter the same file simultaneously

Conflicts can also be minimized with project management strategies:

- Try breaking large files apart into smaller files so that it is less
  likely that you will be working in the same file at the same time
  in different branches
- Create branches focused on separable tasks so that your work won't overlap in files
- Clarify who is responsible for what areas with your collaborators
- Discuss what order tasks should be carried out in with your collaborators so
  that tasks that will change the same file won't be worked on at the same time



:::::::::::::::::::: challenge

## Create a conflict between branches and resolve it

- Create a new branch off of the main branch
- Make a change to a file in the main branch
- Change to the new branch
- Make a change to the same line in the same file
- Change back to the main branch
- Merge the new branch into the main branch
- Address the resulting conflict in the text editor of your choice
- Add the file containing the conflict and commit conflict resolution to the repository

::::::::::::::::::::::::::::: solution
## Solution

```bash
# to make sure we're starting in the main branch
$ git checkout main 
# create a new branch, but don't change into it
$ git branch new_branch 
# make a change to the file
$ nano mars.txt 
# add changes in mars.txt to the staging area
$ git add mars.txt 
$ git commit -m "Small change to mars.txt"
# switch to the new branch
$ git checkout new_branch 
# make a change to mars.txt on the same line
$ nano mars.txt 
# add changes in mars.txt to the staging area
$ git add mars.txt 
$ git commit -m "Another change to mars.txt"
# change back to the main branch
$ git checkout main 
# attempt to merge the branches
$ git merge new_branch 
# address conflicts by removing `<<<`, `===`, and `>>>` lines leaving the desired changes intact
$ nano mars.txt
$ git add mars.txt
$ git commit -m "Resolving conflict in mars.txt."
```

:::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- Conflicts occur when files are changed in the same place in two commits that are being merged.
- The version control system does not allow one to overwrite changes blindly during a merge, but highlights conflicts so that they can be resolved.

::::::::::::::::::::::::::::::::::::::::::::::::::