---
title: "Branches"
teaching: 15
exercises: 0
---

:::::::::::::::::::::::::::::::::::::: questions 

- "What are branches?"
- "How can I work in parallel using branches?"

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- "Understand why branches are useful for:"
- "*working on separate tasks in the same repository concurrently*"
- "*trying multiple solutions to a problem*"
- "*check-pointing versions of code*"
- "Merge branches back into the main branch"

::::::::::::::::::::::::::::::::::::::::::::::::

So far we've always been working in a straight timeline.
However, there are times when we might want to keep
our main work safe from experimental changes we are working on.
To do this we can use branches to work on separate tasks in parallel
without changing our current branch, `main`.

We didn't see it before but the first branch made is called `main`.
This is the default branch created when initializing a repository and
is often considered to be the "clean" or "working" version of a
repository's code.

We can see what branches exist in a repository by typing

```bash
$ git branch
```

```output
* main
```

The '*' indicates which branch we are currently on.

In this lesson, Dracula is trying to run an analysis
and doesn't know if it will be faster in bash or python.
To keep his main branch safe he will use separate branches
for both bash and python analysis.
Then he will merge the branch with the faster script
into his main branch.

First let's make the python branch.
We use the same `git branch` command but now add the 
name we want to give our new branch

```bash
$ git branch pythondev
```

We can now check our work with the `git branch` command.

```bash
$ git branch
```

```output
* main
  pythondev
```

We can see that we created the `pythondev` branch but we
are still in the main branch.

We can also see this in the output of the `git status` command.

```bash
$ git status
```

```output
On branch main
nothing to commit, working directory clean
```

To switch to our new branch we can use the `checkout` command
we learned earlier and check our work with `git branch`.

```bash
$ git checkout pythondev
$ git branch
```

```output
  main
* pythondev
```

::::::::::::::::::::::: callout

We can use the `checkout` command to checkout a file from a specific commit
using commit hashes or `HEAD` and the filename (`git checkout HEAD <file>`). The
`checkout` command can also be used to checkout an entire previous version of the
repository, updating all files in the repository to match the state of a desired commit.

Branches allow us to do this using a human-readable name rather than memorizing
a commit hash. This name also typically gives purpose to the set of changes in
that branch. When we use the command `git checkout <branch_name>`, we are using
a nickname to checkout a version of the repository that matches the most recent
commit in that branch (a.k.a. the HEAD of that branch).

::::::::::::::::::::::::::

Here you can use `git log` and `ls` to see that the history and 
files are the same as our `main` branch. This will be true until
some changes are committed to our new branch.

```bash
$ git log --oneline
```

```output
e98a594 (HEAD -> pythondev, main) Discuss concerns about Mars' climate for Mummy
33d27e2 Add concerns about effects of Mars' moons on Wolfman
7e1e559 Start notes on Mars as a base
```

Now lets make our python script.  
For simplicity sake, we will create an empty file
but imagine we spent hours working on this python script for our analysis.

Use the **"New file" button** to create a new file called `analysis.py`.

Now we can add and commit the script to our branch.

```bash
$ git add analysis.py
$ git commit -m "Wrote and tested python analysis script"
```

```output
[pythondev x792csa1] Wrote and tested python analysis script
 1 file changed, 1 insertion(+)
 create mode 100644 analysis.py
```

Lets check our work!

```bash
$ git log --oneline
```

```output
d5f2565 (HEAD -> pythondev) Wrote and tested python analysis script
e98a594 (main) Discuss concerns about Mars' climate for Mummy
33d27e2 Add concerns about effects of Mars' moons on Wolfman
7e1e559 Start notes on Mars as a base
```

As expected, we see our commit in the log.

Now let's switch back to the `main` branch.

```bash
$ git checkout main
$ git branch
```

```output
* main
  pythondev
```

Let's explore the repository a bit.

Now that we've confirmed we are on the `main` branch again.
Let's confirm that `analysis.py` and our last commit aren't in `main`.

```bash
$ git log --oneline
```

```output
e98a594 (HEAD -> main) Discuss concerns about Mars' climate for Mummy
33d27e2 Add concerns about effects of Mars' moons on Wolfman
7e1e559 Start notes on Mars as a base
```

::::::::::::::::::::::::::::: callout
We no longer see the file `analysis.py` and our latest commit doesn't
appear in this branch's history. But do not fear! All of our hard work
remains in the `pythondev` branch. We can confirm this by moving back
to that branch.

```bash
$ git checkout pythondev
$ git branch
```

```output
  main
* pythondev
```

```bash
$ git log --oneline
```

And we see that our `analysis.py` file and respective commit have been
preserved in the `pythondev` branch.

Checkout the `main` branch again to prepare for creating another new 
branch based on the version history in main. New branches will
include the entire history up to the current commit, and we'd like
to keep these two tasks separate.

```bash
$ git checkout main
$ git branch
```

```output
* main
  pythondev
```

:::::::::::::::::::::::::::::::::

Now we can repeat the process for our bash script in a branch called
`bashdev`.

This time let's create and switch to the `bashdev` branch
in one command.

We can do so by adding the `-b` flag to checkout.

```bash
$ git checkout -b bashdev
$ git branch
```

```output
* bashdev
  main
  pythonndev
```

We can use `git log` to see that this branch is 
the same as our current `main` branch.

```bash
$ git log --oneline
```

```output
e98a594 (HEAD -> bashdev, main) Discuss concerns about Mars' climate for Mummy
33d27e2 Add concerns about effects of Mars' moons on Wolfman
7e1e559 Start notes on Mars as a base
```

Now we can make `analysis.sh` and add and commit it.
Again imagine instead of creating an empty file we worked 
on it for many hours.

Use the **"New File" button** to create a new file called `analysis.sh`

```bash
$ git add analysis.sh
$ git commit -m "Wrote and tested bash analysis script"
```

```output
[bashdev 2n779ds] Wrote and tested bash analysis script
 1 file changed, 1 insertion(+)
 create mode 100644 analysis.sh
```

Lets check our work again before we switch back to the main branch.

```bash
$ git log --oneline
```

```output
dff27fa (HEAD -> bashdev) Wrote and tested bash analysis script
e98a594 (main) Discuss concerns about Mars' climate for Mummy
33d27e2 Add concerns about effects of Mars' moons on Wolfman
7e1e559 Start notes on Mars as a base
```

So it turns out the python `analysis.py` is much faster than `analysis.sh`.

We will merge the `pythondev` branch into our `main` branch via a Pull Request so we can use it for our work going forward.

Before we can create a Pull Request on GitHub, we need to push this branch to the remote repo

Let's checkout & push the pythondev branch:

```bash
$ git checkout pythondev
$ git push
```

Whoops, we got an error:

```output
fatal: The current branch pythondev has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin pythondev

To have this happen automatically for branches without a tracking
upstream, see 'push.autoSetupRemote' in 'git help config'.
```

While our main branch was already on both our local and remote repositories, our pythondev branch is only on our local computer. You can check this by going to GitHub and searching for the pythondev branch - you won't find it!

We need to use the `-u` flag in our command and specify the destination branch.

```bash
$ git push -u origin pythondev
```

```output
Enumerating objects: 12, done.
Counting objects: 100% (12/12), done.
Delta compression using up to 8 threads
Compressing objects: 100% (8/8), done.
Writing objects: 100% (12/12), 1.07 KiB | 1.07 MiB/s, done.
Total 12 (delta 2), reused 0 (delta 0), pack-reused 0 (from 0)
remote: Resolving deltas: 100% (2/2), done.
To https://github.com/vlad/planets.git
 * [new branch]      pythondev -> pythondev
branch 'pythondev' set up to track 'origin/pythondev'.
```

::::::::::::::::::: callout
## The '-u' Flag

You may see a `-u` option used with `git push` in some documentation.  This
option is synonymous with the `--set-upstream-to` option for the `git branch`
command, and is used to associate the current branch with a remote branch so
that the `git pull` command can be used without any arguments. To do this,
simply use `git push -u origin <branch-name>`.

::::::::::::::::::::::::

Now, we can go back to GitHub and verify that we have a new branch named pythondev.

:::::::::::::::::::::::::::::::::::::::: keypoints

- "Branches can be useful for developing while keeping the main line static."

::::::::::::::::::::::::::::::::::::::::::::::::::