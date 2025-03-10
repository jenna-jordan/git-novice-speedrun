---
title: "Tracking Changes"
teaching: 20
exercises: 2
---

:::::::::::::::::::::::::::::::::::::: questions 

- "How do I record changes in Git?"
- "How do I check the status of my version control repository?"
- "How do I record notes about what changes I made and why?"

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- "Go through the modify-add-commit cycle for one or more files."
- "Explain where information is stored at each stage of that cycle."
- "Distinguish between descriptive and non-descriptive commit messages."

::::::::::::::::::::::::::::::::::::::::::::::::

## Mission to Mars

Wolfman and Dracula have been hired by Universal Missions (a space services spinoff from Euphoric State University) to investigate if it is possible to send their next planetary lander to Mars. They want to be able to work on the plans at the same time, but they have run into problems doing this in the past. If they take turns, each one will spend a lot of time waiting for the other to finish, but if they work on their own copies and email changes back and forth things will be lost, overwritten, or duplicated.

## Create a file

First let's make sure we're in the right directory.
You should be in the `planets` directory.

```bash
$ pwd
```

Let's create a file called `mars.txt` that contains some notes
about the Red Planet's suitability as a base. 

Make sure you have the Explorer pane open in VS Code, and click the "New File" button. Name the file `mars.txt`. Click the file to open it in a tab to edit.

Type the text below into the `mars.txt` file:

```output
Cold and dry, but everything is my favorite color
```

Save the file.

## Track Changes to the File

If we check the status of our project again, Git tells us that it's noticed the new file:

```bash
$ git status
```

```output
On branch main

Initial commit

Untracked files:
   (use "git add <file>..." to include in what will be committed)

	mars.txt
nothing added to commit but untracked files present (use "git add" to track)
```

The "untracked files" message means that there's a file in the directory
that Git isn't keeping track of.
We can tell Git to track a file using `git add`:

```bash
$ git add mars.txt
```

and then check that the right thing happened:

```bash
$ git status
```

```output
On branch main

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   mars.txt

```

Git now knows that it's supposed to keep track of `mars.txt`,
but it hasn't recorded these changes as a commit yet.
To get it to do that,
we need to run one more command:

```bash
$ git commit -m "Start notes on Mars as a base"
```

```output
[main (root-commit) f22b25e] Start notes on Mars as a base
 1 file changed, 1 insertion(+)
 create mode 100644 mars.txt
```

When we run `git commit`,
Git takes everything we have told it to save by using `git add`
and stores a copy permanently inside the special `.git` directory.
This permanent copy is called a commit
(or revision) and its short identifier is `f22b25e`
(Your commit may have another identifier.)

We use the `-m` flag (for "message")
to record a short, descriptive, and specific comment that will help us remember later on what we did and why.
If we just run `git commit` without the `-m` option,
Git will launch `nano` (or whatever other editor we configured as `core.editor`)
so that we can write a longer message.

[Good commit messages][commit-messages] start with a brief (<50 characters) summary of
changes made in the commit.  If you want to go into more detail, add
a blank line between the summary line and your additional notes.

If we run `git status` now:

```bash
$ git status
```

```output
On branch main
nothing to commit, working directory clean
```

it tells us everything is up to date.
If we want to know what we've done recently,
we can ask Git to show us the project's history using `git log`:

```bash
$ git log
```

```output
commit f22b25e3233b4645dabd0d81e651fe074bd8e73b
Author: Vlad Dracula <vlad@tran.sylvan.ia>
Date:   Thu Aug 22 09:51:46 2013 -0400

    Start notes on Mars as a base
```

`git log` lists all commits  made to a repository in reverse chronological order.
The listing for each commit includes
the commit's full identifier
(which starts with the same characters as
the short identifier printed by the `git commit` command earlier),
the commit's author,
when it was created,
and the log message Git was given when the commit was created

## Make more changes to the file

Now suppose Dracula adds more information to the file.

Open the `mars.txt` file again (if you closed it), and add a second line:

```output
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
```

Save the file.

When we run `git status` now,
it tells us that a file it already knows about has been modified:

```bash
$ git status
```

```output
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   mars.txt

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

```bash
$ git diff
```

```output
diff --git a/mars.txt b/mars.txt
index df0654a..315bf3a 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1 +1,2 @@
 Cold and dry, but everything is my favorite color
+The two moons may be a problem for Wolfman
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

```bash
$ git commit -m "Add concerns about effects of Mars' moons on Wolfman"
$ git status
```

```output
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   mars.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

Whoops:
Git won't commit because we didn't use `git add` first.
Let's fix that:

```bash
$ git add mars.txt
$ git commit -m "Add concerns about effects of Mars' moons on Wolfman"
```

```output
[main 34961b1] Add concerns about effects of Mars' moons on Wolfman
 1 file changed, 1 insertion(+)
```

Git insists that we add files to the set we want to commit
before actually committing anything. This allows us to commit our
changes in stages and capture changes in logical portions rather than
only large batches.

To allow for this,
Git has a special *staging area*
where it keeps track of things that have been added to
the current changeset
but not yet committed.

:::::::::::::::::::::::::: callout

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

::::::::::::::::::::::::::

![The Git Staging Area](fig/git-staging-area.svg)

## Using the Source Control pane in VS Code

Let's watch as our changes to a file move from our editor
to the staging area
and into long-term storage.
First,
we'll add another line to the file:


```output
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
```

Save the file.

Switch from the Explorer pane view to the Source Control pane view in VS Code.

Under "Changes", you should see your `mars.txt` file. Click the file.

In your Terminal window, type the command:

```bash
$ git diff
```

```output
diff --git a/mars.txt b/mars.txt
index 315bf3a..b36abfd 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1,2 +1,3 @@
 Cold and dry, but everything is my favorite color
 The two moons may be a problem for Wolfman
+But the Mummy will appreciate the lack of humidity
```

The `git diff` command is accomplishing the same thing as clicking on the file under "Changes"

So far, so good:
we've added one line to the end of the file
(shown with a `+` in the first column).
Now let's put that change in the staging area
and see what `git diff` reports:

In the Source Control pane, click the `+` icon next to the `mars.txt` file. 
The `mars.txt` file gets moved to the "Saved Changes" section.

Clicking the `+` icon accomplished the same thing as typing `git add mars.txt` in the terminal.

Let's continue to compare terminal commands with GUI actions. In the terminal, type:

```bash
$ git diff
```

There is no output:
as far as Git can tell,
there's no difference between what it's been asked to save permanently
and what's currently in the directory.
However,
if we do this:

```bash
$ git diff --staged
```

```output
diff --git a/mars.txt b/mars.txt
index 315bf3a..b36abfd 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1,2 +1,3 @@
 Cold and dry, but everything is my favorite color
 The two moons may be a problem for Wolfman
+But the Mummy will appreciate the lack of humidity
```

it shows us the difference between
the last committed change
and what's in the staging area.

Try clicking the `mars.txt` file under "Staged Changes". What do you see? Which command matches the visual output?

Now, let's save our changes. We could use the Terminal to commit our changes:

```bash
$ git commit -m "Discuss concerns about Mars' climate for Mummy"
```

```output
[main 005937f] Discuss concerns about Mars' climate for Mummy
 1 file changed, 1 insertion(+)
```

Instead, type the commit message "Discuss concerns about Mars' climate for Mummy" in the "Message" box and click "Commit".

Let's check our status:

```bash
$ git status
```

```output
On branch main
nothing to commit, working directory clean
```

and look at the history of what we've done so far:

```bash
$ git log
```

```output
commit 005937fbe2a98fb83f0ade869025dc2636b4dad5
Author: Vlad Dracula <vlad@tran.sylvan.ia>
Date:   Thu Aug 22 10:14:07 2013 -0400

    Discuss concerns about Mars' climate for Mummy

commit 34961b159c27df3b475cfe4415d94a6d1fcd064d
Author: Vlad Dracula <vlad@tran.sylvan.ia>
Date:   Thu Aug 22 10:07:21 2013 -0400

    Add concerns about effects of Mars' moons on Wolfman

commit f22b25e3233b4645dabd0d81e651fe074bd8e73b
Author: Vlad Dracula <vlad@tran.sylvan.ia>
Date:   Thu Aug 22 09:51:46 2013 -0400

    Start notes on Mars as a base
```

Look back at the Source Control pane. Where can you see a GUI representation of this log?

:::::::::::::::::::::::::::::::::::::::: keypoints

- "`git status` shows the status of a repository."
- "Files can be stored in a project's working directory (which users see), the staging area (where the next commit is being built up) and the local repository (where commits are permanently recorded)."
- "`git add` puts files in the staging area."
- "`git commit` saves the staged content as a new commit in the local repository."
- "Always write a log message when committing changes."

::::::::::::::::::::::::::::::::::::::::::::::::::

[commit-messages]: https://chris.beams.io/posts/git-commit/