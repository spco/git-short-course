---
title: Branching
teaching: 25
exercises: 0
questions:
- "How can I use branching to make my workflow more robust?"
objectives:
- "Create a new branch in a repository."
- "Create and merge a Pull Request on GitHub."
keypoints:
- "`git branch branch-name` creates a new branch based at the current location."
- "`git push --set-upstream origin branch-name` sets `origin/branch-name` as the upstream branch of `branch-name`."
---

For this next step, you will work on your own, but bear in mind that what is learnt
can equally be applied to another user's repository  e.g. via GitHub. The goal is that
you create a new branch for a new feature, and then use GitHub's interface to merge 
the changes from that branch into the master.

Firstly, ensure you are working on _your_ repository:
~~~
$ cd ~/git/planets
~~~
{: .bash}

Next, ensure that you have the very latest version from GitHub:
~~~
$ git checkout master
$ git fetch origin master
$ git pull origin master
~~~
{: .bash}

To check that everything is up-to-date, we can use lots of options added to the basic 
`git log` command to show us more detail:
~~~
$ git log --oneline --branches --graph --decorate
~~~
{: .bash}

~~~
* 83f3f03 (HEAD -> master, origin/master) Discuss concerns about Mars' climate for Mummy
* 2e764a3 Add concerns about effects of Mars' moons on Wolfman
* 0f787d0 Start notes on Mars as a base.
~~~
{: .output}
This shows us that `HEAD` is at `master` (denoted by the arrow), and `origin/master` is 
at the same point in history.

Now that we're happy that we're up to date, we're going to make a new branch that is 
_based_ on the latest commit on master.
~~~
$ git branch new_branch
~~~
{: .bash}
Nothing looks to have changed, but if we again use `git log --oneline --branches --graph --decorate`
we see that `new_branch` is now added. 
~~~
* 83f3f03 (HEAD -> master, origin/master, new_branch) Discuss concerns about Mars' climate for Mummy
* 2e764a3 Add concerns about effects of Mars' moons on Wolfman
* 0f787d0 Start notes on Mars as a base.
~~~
{: .output}
But be careful! If we run `git status` we see `On branch master`. So we have created a new branch 
called `new_branch`, but are not on the branch yet. The `git log` command above also indicates this,
because `HEAD` is still pointing to `master`, not to `new_branch`.

We check out the new branch:

~~~
$ git checkout new_branch
~~~
{: .bash}

~~~
Switched to branch 'new_branch'
~~~
{: .output}

~~~
$ git log --oneline --branches --graph --decorate
~~~
{: .bash}

~~~
* 83f3f03 (HEAD -> new_branch, origin/master, master) Discuss concerns about Mars' climate for Mummy
* 2e764a3 Add concerns about effects of Mars' moons on Wolfman
* 0f787d0 Start notes on Mars as a base.
~~~
{: .output}

We're now on the new branch, and so `HEAD -> new_branch`.

Time to make some changes!

Make some changes to mars.txt, or perhaps add a new file. Then add and commit these changes. In
this example, I've added notes on Jupiter to `jupiter.txt`:

~~~
* ef25bc8 (HEAD -> new_branch) Added notes ruling out Jupiter as a destination.
* 83f3f03 (origin/master, master) Discuss concerns about Mars' climate for Mummy
* 2e764a3 Add concerns about effects of Mars' moons on Wolfman
* 0f787d0 Start notes on Mars as a base.
~~~
{: .output}

Note that this latest commit is on `new_branch`, but it hasn't affected `master`.

Let's use GitHub's Pull Request mechanism to merge these changes into `master`.

~~~
$ git push
~~~
{: .bash}
~~~
fatal: The current branch new_branch has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin new_branch
~~~
{: .output}
What's happened here? git doesn't know which branch to push our changes to in the
GitHub repository (the "upstream branch"). But it suggests what it thinks is most 
likely, and it's what we want in this case, so we go ahead and follow the advice:
~~~
$ git push --set-upstream origin new_branch
~~~
{: .bash}
~~~
Counting objects: 3, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 365 bytes | 365.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To github.com:spco/planets.git
 * [new branch]      new_branch -> new_branch
Branch new_branch set up to track remote branch new_branch from origin.
~~~
{: .output}


> ## A Basic Collaborative Workflow
>
> In practice, it is good to be sure that you have an updated version of the
> repository you are collaborating on, so you should `git pull` before making
> our changes. The basic collaborative workflow would be:
>
> * update your local repo with `git pull origin master`,
> * make your changes and stage them with `git add`,
> * commit your changes with `git commit -m`, and
> * upload the changes to GitHub with `git push origin master`
>
> It is better to make many commits with smaller changes rather than
> of one commit with massive changes: small commits are easier to
> read and review.
{: .callout}

> ## Switch Roles and Repeat
>
> Switch roles and repeat the whole process.
{: .challenge}

> ## Review Changes
>
> The Owner pushed commits to the repository without giving any information
> to the Collaborator. How can the Collaborator find out what has changed with
> command line? And on GitHub?
>
> > ## Solution
> > On the command line, the Collaborator can use ```git fetch origin master```
> > to get the remote changes into the local repository, but without merging
> > them. Then by running ```git diff master origin/master``` the Collaborator
> > will see the changes output in the terminal.
> >
> > On GitHub, the Collaborator can go to their own fork of the repository and
> > look right above the light blue latest commit bar for a gray bar saying
> > "This branch is 1 commit behind Our-Repository:master." On the far right of
> > that gray bar is a Compare icon and link. On the Compare page the
> > Collaborator should change the base fork to their own repository, then click
> > the link in the paragraph above to "compare across forks", and finally
> > change the head fork to the main repository. This will show all the commits
> > that are different.
> {: .solution}
{: .challenge}

> ## Comment Changes in GitHub
>
> The Collaborator has some questions about one line change made by the Owner and
> has some suggestions to propose.
>
> With GitHub, it is possible to comment the diff of a commit. Over the line of
> code to comment, a blue comment icon appears to open a comment window.
>
> The Collaborator posts its comments and suggestions using GitHub interface.
{: .challenge}

> ## Version History, Backup, and Version Control
>
> Some backup software can keep a history of the versions of your files. They also
> allows you to recover specific versions. How is this functionality different from version control?
> What are some of the benefits of using version control, Git and GitHub?
{: .challenge}
