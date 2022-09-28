# git and GitHub

## What is git?

A free, open-source *version control system* for tracking changes to code (and other files). Think of it like track changes in Word, or the version history in Google Docs. It keeps a record of changes you make to your files. It allows you to see the history of changes you've made, compare file versions, and to restore previous versions of your code if you introduce new bugs. Though useful by itself, it really shines when you combine it with GitHub.

## What is Github?

GitHub is a website where you can store git repositories, share them with others, and collaborate on code. It's a little like Dropbox, Box, or Google Drive, but specialized for working with git repos. It requires you to create an account (which is free), though they also have some paid plans that offer various nice features. 

## Interacting with git/Github

You can work with git in a number of ways: it is a command line program, so you can do everything from the terminal. All commands take the form `git command -flags options`. I find it easier to do most things in a GUI, of which there are many available. RStudio has some built in git functionality if you want to stay within one program, as does Visual Studio Code. I also like GitKraken, which is a paid app but has free licenses for students and educators. For today, I'll show you a little of everything. 

## Before we begin...

Learning git can be challenging! It is a powerful tool, which means it can also be very complex. It also has a lot of jargon and terminology that can be strange and unintuitive. So, don't feel discouraged if you don't initially understand something, or run into some weird issues or error messages (we'll probably run into some today). We'll try to work through any issues together today, and when you're working on your own the usual strategies of Google and stackoverflow will work well for fixing problems: people have been using git for many years, it is likely someone has faced your problem before.

Learning git takes some work, but it is worth it (there's a reason that most of the R packages we use are developed on git and Github)!

## Starting a git/GitHub repositoy

Let's walk through making a git/GitHub repository. There are a few ways to do this, but the easiest is to start the repo on Github. So, go to Github.com, sign in if you aren't already, and click "New". Give the repo a name. I recommend creating the repo with a README: it's good to have a README no matter what, and adding one now will automatically "initialize" the repository (which is a concept I'm going to skip over for now). Then, click create repository. 

At this stage, the repo exists and has been initialized, but the files are only on GitHub. We need to copy them down to our computer.

### Copy repository: `git clone`

In your terminal, navigate to the place where you want your repo to be saved. Copy the repo URL from GitHub via the green "Code" dropdown menu. Then, in your terminal, run:

`git clone https:://github.com/user/repo.git`

Replacing the URL with the one you copied. For public repositories, that should be it. For private repositories, you may have to enter your username and password or personal access token.

And that should do it! You should have on your computer some (probably empty) files that are the same as in your repo on GitHub. The big advantage of this approach, where you "clone" a repository from GitHub, is that it automatically links the git repo on your computer with the git repo on GitHub: when it comes time to sync the two (which we'll get to), you should already be set up. If you start your repository in other ways, you have to do a little setup (which we won't worry about for today). 

## A simple(?) workflow for working alone

Once you have a git repository, working with it is conceptually straightforward (though not always straightforward in practice). You:

1. Make changes to your files, saving them to your computer as normal. 
2. Once you have made enough changes that you want to save the state of your files, tell git which files you want to save by "staging" them.
3. Then, tell git to save those changed by making a "commit" with a descriptive message. 
4. Periodically sync the files on your computer with the files on Github. 

One major conceptual point to keep in mind is that git does not track your files constantly. It is more like a checkpoint system: when you tell it to, it saves/remembers the current state of files, and makes that a checkpoint you can return to in the future. These checkpoints are called "commits". You could also think of commits as snapshots, or versions. The main point to remember is that git does not save EVERY change you have ever made to a set of files: it only remembers the changes you tell it to remember.

Anyway, go make some changes to your files, and then we'll explore the next steps of the workflow.

### See the current state: `git status`

`git status` tells you what files have changed or been added/deleted since the last commit, and whether any of those changes are "staged", which we will return to. This is a place where I think GUIs are useful: Rstudio, VSCode, and GitKraken all have nice displays of which files have changed since the last commit, and usually ones that update automatically.

### See specific change(s): `git diff`

`git status` only lists which files have changed since the last commit, it does not say HOW they have changed. The `git diff` command does that (it is similar to the unix `diff` that lets you compare files). Here again, I like GUIs better for this, and rarely bother with actually using `git diff`.

### "Stage" file(s): `git add`

Next, you tell git which changes/files you'd like to include in your next commit. In the command line, you use the `git add` command to stage changes. Again, the GUIs can be easier for this, as you can just point and click to add files.

One thing to note: if you stage the changes in a file, and then make additional changes, you'll have to stage the file again for that second round of changes to be included in the commit.

#### Sidenote: .gitignore

This is a text file where you list files for git to ignore. These files won't be added to your git repo, and changes in these files won't be tracked. You can use some wildcards to specify multiple file names at once. 

Things to ignore:
* Large files: GitHub has a soft limit of 50MB for files, and a hard limit of rejecting pushes >100MB. If repos get too large (i.e., a few GB) you may get warnings from GitHub.
* Local config files (e.g., .Rproj.user, bash/sh histories, and config settings you don't want to share).
* Security files: personal access tokens, SSH keys, passwords, etc. GitHub has some nice automated features for detecting and removing some types of keys if you accidentally commit and upload them, but better to just never commit these in the first place. 
* Locally installed software (e.g., if you have a local bin/ folder in your repo).
* Files that can easily be regenerated (e.g., .html and .pdf reports from RMarkdown).
* Some types of binary files (.xlsx, .pdf). Sometimes it can be useful to track these, just to see if they have changed. But `git diff` will not be useful in seeing what has actually changed. 

### Save your staged changes: `git commit`

Once you've made the changes you want and staged them, you commit them to create your checkpoint/snapshot. You can use the `git commit` command, but again the GUIs have nicer options for writing nice commit messages. This is an important part of git: the commit messages provide information and context for you when you're going back and looking through your old code. So strive to write commit messages that are informative and concise. 

How often should you commit your changes? This is mostly up to you in terms of personal style and the culture/expectations of the team you're working with. I generally try to keep commits focused on one issue or theme, so that the changes are clearer. Lots of small commits could be a little annoying, but I would err on the side of too many commits than too few. A big, cumbersome commit with hundreds of file changes can be hard to understand. 

### Sync your changes: `git push` and `git pull`

So, you've made a bunch of changes and had git remember them. At the moment, these changes are only present on your computer: git isn't like Dropbox, it doesn't automatically sync file changes between your computer and Github. You have to do that yourself, using two commands: `git push` sends changes from your computer to GitHub, and `git pull` retrieves changes from GitHub and puts them on your computer (technically, `git pull` combines two commands into one: `git fetch` to check for new changes, and `git merge` to incorporate those new changes). As a bit of git terminology, the repo on github is called a "remote". 

When you're working by yourself on a single computer, you should only need to push your changes. In that case Github is basically just another backup of your code that others can access. Once you start working across multiple computers, you need to be a little more careful. A general piece of advice is to run `git pull` often to make sure your local repo has all the latest changes from GitHub/the remote. To explain why consider this scenario:  

Say you have two clones of a repo: one on your laptop, and one on Grace. One day you edit the repo on your laptop, commit some changes, and push it to GitHub. Then, a few days later, you go to work on the same repo on Grace, and edit the same file, but without pulling the changes from GitHub first. When you go to push your commit to GitHub, it will be rejected: git doesn't know which version of the file to use: the changes you made on your laptop, or the changes you made on Grace. 
 
This is called a "merge conflict". We'll talk more about merge conflicts later, when we talk about collaborating. For now, I'll just say that an ounce of prevention is worth a pound of cure. When working across multiple computers, its best to always run `git pull` to check for and incorporate any recent changes from the remote branch before you start making more changes. 

Finally, a little note/tip for pushing: you don't need to push every commit immediately. In some cases, it is nice to wait a little bit: if you, say, discover a typo in your code or commit message you can fix that (check out `git commit --amend`) if your commit hasn't been pushed yet. But once a commit has been pushed to Github you can't easily edit it anymore. 

### Repeat

That basic workflow is enough to get you started. If you're working on one computer and working by yourself, that basic workflow of (1) making changes, (2) staging them, (3) committing them, and (4) pushing them to GitHub is all you need to share your code with others on GitHub. 

If we have time, experiment a little with this workflow. Then, we'll move on to a couple more advanced concepts that will allow us to  work collaboratively on projects. 

## Concept: branches

So far, we've glossed over an important concept in git: branches. As we said before, commits are checkpoints/snapshots of your files at different states. As you move forward and make commits, you're leaving behind a history of changes that all depend on each other. Here some of GitKraken's visualizations are useful: we can see the line of commits going back in time. This is a branch, and we've been working on one branch (main/master) this whole time. 

But, we can have more than one branch: more than one path/flow of development that we're working on. This is super-useful for collaboration: changes that are happening on one branch are independent of changes happening on another branch. This makes it easy to, say, have two different people working on the repository at the same time. Or, to maintain a bug-free, stable branch of your software that users can download, while you add new features and test things out on different branches. 

### Create a branch: `git branch`

We can create a new branch with `git branch name_of_new_branch`. Then, we can switch to the new branch by "checking it out": `git checkout name_of_new_branch`. You can also create and checkout a branch all at once: `git checkout -b name_of_new_branch`. All the GUIs we've mentioned also have methods for making new branches. 

You can switch between branches with `git checkout`, but switching branches when you have lots of uncommitted work can sometimes be tricky. git has some tools for dealing with this (`git stash`, which we won't be getting into today), and Gitkraken can sometimes apply these tools automatically. But, in general, if you have a lot of uncommitted work in one branch and you want to switch to working on a different branch, you should commit your work first. 

## Concept: merging

Of course, we probably don't want to have two different versions of our code indefinitely: eventually, we want to combine branches, so that the changes made in one branch can be incorporated into a different branch. In git parlance, this is called "merging".

### Merge a branch `git merge`

Say you've been working on a branch, "new_feature", and you want to incorporate it into your main branch (probably called "main" if you set up your repo on Github, or "master" if you did it from the command line). To do this, you would checkout the main branch with `git checkout main`, and then merge in the feature branch with `git merge new_feature`. If you want, you can then delete the feature branch with `git branch -d new_feature`, though depending on your workflow you may want to keep it. 

## Merge conflicts

In most cases, this merge process is automatic and painless: git can easily figure out how to incorporate your changes. As we alluded to above, though, in some cases the merge might not be possible. This usually happens when the main branch has been changed since you branched from it. That is, if you made the new_feature branch at commit A, and have since done commits B and C, while the main branch has had new commits X and Y that affect the same files, git won't know how to reconcile those differences. 

When this happens, git will abort the merge. Depending on how you're interacting with git, different things may happen. Some programs (GitKraken) have integrated tools to help with merge conflicts. If you're working from the terminal, you'll need to use `git status` to figure out which file(s) are causing the conflict. Then, you'll need to resolve the conflict: we can look at an example here: https://happygitwithr.com/git-branches.html?q=merge#merging-a-branch

One of the more confusing things that can happen is, after you fix the conflict (and especially when working in the terminal), git may open up your default terminal text editor and ask you to write a merge message (like a commit message). This is usually made extra confusing if you rarely use the default terminal text editor. It is likely a program called Vi or Vim, and it likely uses some hot keys that you don't know, making it hard to navigate, save your merge message, and exit. If that happens, check out this Stackoverflow answer: https://stackoverflow.com/questions/19085807/please-enter-a-commit-message-to-explain-why-this-merge-is-necessary-especially

Again, in general, its best to avoid merge conflicts by: (1) running `git status` or equivalent frequently, so you know what is going on, (2) pulling new changes into your work frequently (we'll talk about how to do this in our collaborative setting soon), and (3) doing most of your work in independent feature branches, and rarely making commits in "main". That is, most of the changes that occur in the main branch should be happen via merges/pull requests from other branches.

With those concepts out of the way, let's (finally!) talk about how to collaborate on a shared repository. 

## A simple(?) workflow for working together

The workflow model we'll use is called sometimes called "fork-branch-pull" or "fork-and-clone". We'll follow along from this tutorial (https://www.tomasbeuzen.com/post/git-fork-branch-pull/), though this general workflow is described elsewhere as well (e.g., https://happygitwithr.com/fork-and-clone.html#fork-and-clone). This is also (more or less) the workflow used for QIIME 2 development. We will:

1. On Github, make a personal copy of an existing repo owned by someone else ("fork").
2. Copy our personal copy down to our computer, where we can work on it.
3. Configure our local copy so that it can pull changes from the original repository. 
4. Make our changes in a new branch, which we can sync up with our copy of the repo.
5. Request that our changes be added to the original repository by opening a "pull request", which the owner of the original repo can accept or reject.
6. Optionally, delete our branch/fork.

### Make a personal copy of someone else's repo ("fork")

On GitHub, navigate to the repo you want to contribute to, then click "fork" in the upper right hand corner. Fork the repo to your personal github account (easiest to just keep the repo name the same). 

### Get your new personal copy to your local computer ("clone")

We've done this once before. See if you can remember, or refer to the git clone section above. 

### Set up the remotes on your local repo

We've cloned the local copy of our repo from our personal copy on our Github: this automatically set up our personal GitHub repo as a "remote" repository that we can push to and pull from. But we also want/need to be able to pull changes from the original repository while we're working on our local copy. That way, we can incorporate changes from the original repository, hopefully making it easy to merge later on down the line. 

So, we'll set the ORIGINAL repository as an "upstream remote". Go to the original repo and copy the URL for the repo (same as you did for cloning in the first section). Then, add the original repo as an upstream remote with:

`git remote add upstream https://github.com/user/orginal_repo.git`

You can double check this worked with `git remote -v`, which lists your current remotes. You should see the URL for your personal fork with "origin" in front of it, and the URL for the proteomicsDIA fork with "upstream" in front of it. 

### Make a branch in your local repo

Now, you can make a branch in your local repo (maybe named after the issue you'll be working on) and make changes to the code in that branch as normal. You can commit those changes, push them to your personal copy of the repo, and generally work with your "fork" as if it were your own (which it is). The main difference is that, you'll want to periodically check to see if the original repository has been updated, and incorporate those changes if so. 

### Incorporate changes from the original repo

To do that, switch to the main branch of your local repo (`git checkout main`). Then, we can check on our remote upstream branch and see if there have been any changes:

`git fetch upstream main`

Once we do that, we can run `git status`: if it says our branch is "behind" the upstream branch, then we can incorporate those changes:

`git pull upstream main`.

Then, switch to the feature branch you're working from (`git checkout new_feature`), and merge in the changes from your local master branch (`git merge main`). You may have to deal with some merge conflicts here. But better to do this often, and deal with merge conflicts when they are small, than to never pull any updates and run into big issues later! 

At this point, we've incorporated the changed from the original repo into (1) our local main branch, and (2) our local feature branch. If we want to keep the remote main branch of our git repo up-to-date (which is not necessary, but maybe you like keeping it all synced), then switch to the main branch and push it to your personal repo:

`git checkout main`
`git push origin main`

Now, everything should be nice and in-sync, and you can go back to working on your feature branch and making all your code changes. Once you've made all the changes you want and are ready for them to be integrated into the original repo, commit all your changes and push them to your GitHub fork. 

### Request for your changes to be incorporated

So, we've made some changes to our personal copy, and we want to add those to the original repo. We can do that by making a "pull request". This is a GitHub feature where we ask the owner of a different repository to pull in our changes. Depending on how access permissions are set up on the repo, you can start a pull request a few different ways.

We'll assume that you don't have permissions for the original repo. Go to your fork, and you'll either see a new option to make a pull request, or you may have to click on your branches and start one from there. Like with a commit, you'll want to provide a good message. A nice trick: if your PR fixes an existing issue, you can write "fixes #XX" or "closes #XX" in your pull request message, and GitHub will automatically close the issue for you when it is merged. 

When you complete a PR, your code isn't automatically incorporated: the owner of the repository has a chance to check out your code, see if they want to included it, ask you to make changes, etc. GitHub has a lot of nice features that can be used to perform automated checks on the code in a pull request: it can not only check whether git can successfully merge the code, but you can set up many other things as well (e.g., can check the format of the code, or run R CMD CHECK to see if the package is intact). 

Once the PR is accepted, that feature branch can be deleted, both from your local copy and from your fork.

### Repeat

If you want to do more work on the repo, you can do a couple things. If you have no uncommitted changes, you could just delete your fork altogether and make a new one, which will now incorporate your new changes. This might be a good idea in a really active repository that you won't be returning to for a while: if you don't want to start work on it again immediately, better to just re-fork later rather than be constantly pulling in new changes. 

In our case, it may be better to just re-sync your local fork with the remote one. You can pull in the changes to your local main branch, as discussed above:

`git checkout main` # check out local  main branch
`git fetch upstream main` # check for changes from upstream
`git pull upstream main` # incorporate upstream changes

Then, you may want/need to push your these changes from your local main branch to the main branch of your GitHub: `git push origin main`. But again, that is sort of optional: you do all your branching from the local version of main. 

### Reviewing a PR

Say you're the maintainer of repository and someone has just made a pull request. You looked at the code online and it seems fine, but you want to double-check it by actually running the code on your computer. There are ways to do this! The GitHub command-line interface, which we haven't talked about, provides an option to do this. But you can also do it with regular git.

To make this more explicit, let's assume we're working on the same project we've been discussing throughout this workshop. There's a "primary" repository that you don't own, and you have a fork of it in your personal GitHub and have set the primary repo as an upstream remote to your fork. Someone else, with username "coder_X", has their own personal fork of the primary repository and implemented a feature in a branch called "feature_branch". They make a pull request, number 99, for that branch against the primary repository. You'd like to download that code on your computer and check it out. First, you'd fetch the changes:

`git fetch upstream/URL pull/99/head:coder_X/feature_branch`

This would let you see the changes in the git history. To actually incorporate the changes, make a new branch (maybe, PR_checking?):

`git branch PR_checking`

Checkout that branch, and then pull/merge in the changes:

`git pull upstream/URL pull/99/head:coder_X/feature_branch`

What if you discover some changes you'd like to make. In general, the best thing to do is to just use the GitHub review features to ask coder_X to make changes. If you really want, you could commit changes on your branch (PR_checking), push them up to your own fork on GitHub (`git push origin/PR_checking`), and then open a full request against the feature_branch in coder_X's fork of the repo. Then, coder_X could incorporate those changes, and those changes would also be reflected in their pull request against the "primary" repository.  


## Further resources

There's a wide world of git out there awaiting you. Here are some resources that might be helpful:

https://github.com/k88hudson/git-flight-rules - A set of "flight rules", basically SOPs/checklists, of how to do various things in git. Focused on command line usage, but lots of very useful things in here. 

https://ohshitgit.com - A little like the "flight rules", but with descriptions written more in plain language, less in git jargon. Can be helpful when you're trying to fix something, but don't quite know the git lingo for what you want to do. 

https://happygitwithr.com/index.html - A very R-focused tutorial on using git and GitHub. Great if you want to mostly use the RStudio GUI, and some R packages that wrap some git functionality and make it a little easier to work with. It also has some really good workflows and advice about how to fix mistakes. 

https://ohmygit.org -  This is a game that is supposed to help you visualize git structures and learn git. I have never played it, no idea if it is actually worth it.

There are many more resources and explanations at the official git website: https://git-scm.com. Sometimes these are very useful, and sometimes they might be a little too deep in the jargon for newcomers. They have a free online book that has lots of useful material (e.g., https://git-scm.com/book/en/v2/Getting-Started-What-is-Git%3F). 

