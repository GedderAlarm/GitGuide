# Git Guide

If you are well aware of the difference between a **distributed** and **centralized** version control system, skip to the beef at [Workflows](#workflows).

-  [Introduction](#introduction)
-  [Workflows](#workflows)
  -  [Centralized Workflow](#centralized-workflow)
  -  [Feature Branch Workflow](#feature-branch-workflow)

## Introduction

Git is what we call a **distributed version control system (DVCS)**.

Github, on the other hand, is a third-party service that provides (free and paid) Git repository hosting services to the public. *It is completely separate from Git* ([so much so that the developer of Git isn't so fond of Github](https://github.com/torvalds/linux/pull/17#issuecomment-5654674)). There are many, many other such hosting services. In fact, you can just as well run a git *host* on your own private server.

For this organization's purposes, we will currently primarily focus on using Github's hosting services. Workflow is, however, maintained through the git *tool*, and it is not recommended (at least in the long run) to work *through* Github (i.e. don't edit, commit, etc., through Github's web GUI).

Github should primarily be used for the following:

-  Issue Tracking
-  Pull Requests
-  Hosting live Repositories

and so on.

### Why is it "distributed"?

Git contrasts with other very popular version control systems like Subversion, which we say is a **centralized version control system (CVCS)**.

A centralized VCS works rather intuitively and straightforwardly:

1.  Download the file you want to change from the server. (Checkout)
2.  Make some changes to the file you downloaded. (Edit)
3.  Commit the edited file to the server. (Checkin)

This Checkout-Edit-Checkin workflow is simple, but comes with a host of limitations that distributed VCSs try to solve. (We won't go into the details of those limitations.)

A distributed VCS workflow tends to be something like this:

1.  Edit your local file (Edit)
2.  Merge any changes from upstream (Merge)
3.  Commit the total changes (Commit)

Since repositories are distributed, many other people may be working on the same file as you simultaneously and it would be necessary to **merge** their changes if they committed before you. Then your set of files are officially up-to-date, and can be safely committed (and in git terminology, "pushed") upstream.

And this is one major reason it's distributed, or *de*centralized: many people may have copies of the repository on their local machines, all simultaneously working with potentially the same file(s).

The tale is longer than this, but you'll get the gist of it as you read on.

## Workflows

All version control software worth its salt comes with some form of branching mechanism. How they handle them is different.

For example, Subversion hosts branches *within* the overall repository's folder structure. What we call the `master` branch in git, we call the `trunk` folder in Subversion. What we may call the `dev` branch in git, is what we may call a subfolder inside of the `branches` folder in Subversion. This is not a Subversion lesson, but it helps to understand that VCSs come in many flavors with their own ways of handling a fundamental version control concept.

But when branches come into play in a distributed system like git, it helps to have an agreed upon workflow for the team in question. Otherwise, everyone will be stepping over each other's toes.

Most importantly, grasping the branching concept gives you great developmental power and efficiency; although there's the overhead to learn it, the results are quite worth it.

### Centralized Workflow

Although git is distributed, it is easy for one to adopt a simple, centralized workflow akin to what users of Subversion use. In fact, most people who work alone using git as their version control software, follow this workflow, more or less.

The Centralized Workflow is best demonstrated through an example:

#### An Example

John is in New York working for a company hosted somewhere in the middle of the USA. Steve lives in California and is also hired as a remote software developer for the same company. They are assigned to the same project.

> John: Let's start our new assignment.
>
> John: `git init AwesomeNewProject`
>
> John: `cd AwesomeNewProject`
>
> John: `cat "# Awesome New Project" >> README.md`
>
> John: Alright, this looks ready to go!
>
> John: `git add README.md`
>
> John: `git commit -m 'Initial commit'`
>
> John: `git remote add github git@github.com:John/AwesomeNewProject.git`

John's setup a project, and in the last step, he connected himself to Github's servers for some upcoming upstream action:

> John: `git push github master`
>
> John: Alright, my repo's live!

And in fact it would be, in particular at www.github.com/John/AwesomeNewProject. But don't look there, this is a fake story.

Meanwhile, a few thousand miles away:

> Larry: I hope John started that project. He told me over the phone that it'd be named 'AwesomeNewProject' under his profile on Github. Let's see if we can get it.
>
> Larry: `git clone git@github.com:John/AwesomeNewProject.git` (success)
>
> Larry: Great, it worked!
>
> Larry: `cd AwesomeNewProject && ls`
>
> Larry: Just a `README.md` file... alright let's get something started.
>
> Larry: `vim main.cpp`
>
> Larry: ...
>
> Larry: Ok, done. Let's push this.
>
> Larry: `git add -A`
>
> Larry: `git commit -m 'Skeletal main.cpp'`
>
> Larry: `git push -u origin master`

Note that Larry calls his remote "origin" whereas John called his remote "github". This literally doesn't matter, it's just about what that remote *references*. In this case, both of them reference `git@github.com:Larry/AwesomeNewProject.git`.

John comes back into the picture:

> John: I should probably add some detail to that `README.md` file.
>
> John: `vim README.md`
>
> John: ...
>
> John: Ready to go. Let's do this.
>
> John: `git commit -a -m 'Adding some basic info on the project'`
>
> John: `git push github` (fail)
>
> John: What's that? Oh, sneaky Larry. He started work on this before me. Alright, let's grab his work.
>
> John: `git pull github master` (success, no conflicts)
>
> John: Oh nice, he already started on `main.cpp`! We're well on our way.

Since Larry already committed his changes to the same branch John is working on (`master`), git disallowed John from pushing unless he caught up ("pull"ed) with the most recently pushed commits from others. Once he pulled Larry's additions, git no longer complained.

### Feature Branch Workflow
