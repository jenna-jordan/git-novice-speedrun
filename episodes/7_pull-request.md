---
title: "Pull Requests"
teaching: 15
exercises: 0
---

:::::::::::::::::::::::::::::::::::::: questions 

- "What are pull requests for?"
- "How can I make a pull request?"

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- "Define the terms fork, clone, origin, remote, upstream"
- "Understand how to make a pull request and what they are useful for"

::::::::::::::::::::::::::::::::::::::::::::::::


:::::::::::::::: callout

Pull requests are a great way to collaborate with others using github.
Instead of making changes directly to a repository you can suggest changes to a repo.
This can be useful if you don't have permission to modify a repository directly or
you want someone else to review your changes.

:::::::::::::::::::::

On GitHub, in your `planets` repo, click on the "Pull Requests" tab. 

Then click "New pull request". Alternatively, GitHub will see your new branch with recent changes and will prompt you to "Compare & pull request". Click this button to also be taken to the new pull request page.

Make sure that the "base" branch is `main` and the "compare" branch is `pythondev`.

Next, we need to give our PR a title. By default, your PR will get the title from your last commit. We can leave this as is.

We can leave a note in the PR description, like:

"The python analysis was faster than the bash analysis, so I am merging only the python analysis to main".

Click "Create pull request".

Your planets repo should now have 1 pull request. This is the phase where you would normally request a reviewer, who can leave comments on your code. In repos that you do not own, your PR will need to be approved by a codeowner before you can merge it. Since you are the codeowner, we can go ahead and click "Merge pull request".

Go back to the "Code" tab and make sure you are on the "main" branch. You should now see an "analysis.py" file.

Let's go back to VS Code. Checkout the main branch, and look at the files and history:

```bash
$ git checkout main
$ git log --one-line
```

```output
fill in later
```

The analysis file is not on "main" yet. This is because we need to first pull the changes we made with the Pull Request from the remote repository.

```bash
$ git pull
$ git log --one-line
```

```output
fill in later
```

Now our main branch is up to date.

## Deleting branches
Now that we've merged the `pythondev` into `main`, these changes
exist in both branches. This could be confusing in the future if we
stumble upon the `pythondev` branch again.

We can delete our old branches so as to avoid this confusion later.
We can do so by adding the `-d` flag to the `git branch` command.

```bash
git branch -d pythondev
```

```output
Deleted branch pythondev (was x792csa1).
```

And because we don't want to keep the changes in the `bashdev` branch,
we can delete the `bashdev` branch as well

```bash
$ git branch -d bashdev
```

```output
error: The branch 'bashdev' is not fully merged.
If you are sure you want to delete it, run 'git branch -D bashdev'.
```

Since we've never merged the changes from the `bashdev` branch,
git warns us about deleting them and tells us to use the `-D` flag instead.

Since we really want to delete this branch we will go ahead and do so.

```bash
git branch -D bashdev
```

```output
Deleted branch bashdev (was 2n779ds).
```

Finally, we can also delete the pythondev branch on GitHub. Click on "Branches", and to delete the `pythondev` branch click the trashcan icon to the right of the branch name.

:::::::::::::::::::::::::::::::::::::::: keypoints

- "Pull requests suggest changes to repos where you don't have privileges"

::::::::::::::::::::::::::::::::::::::::::::::::::