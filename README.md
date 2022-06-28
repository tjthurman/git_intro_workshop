# Git and GitHub

## What is Git?

A free, open-source *version control system* for tracking changes to code (and other files). Think of it like track changes in Word, or the version history in Google Docs. It keeps a record of changes you make to your files. It allows you to see the history of changes you've made, compare file versions, and to restore previous versions of your code if you introduce new bugs. Though useful by itself, it really shines when you combine it with GitHub.

## What is Github?

GitHub is a website where you can store Git repositories, share them with others, and collaborate on code. It's a little like Dropbox, Box, or Google Drive, but specialized for working with Git repos. It requires you to create an account (which is free), though they also have some paid plans. 

## Interacting with Git/Github

You can work with git in a number of ways: it is a command line program, so you can do everything from the terminal. All commands take the form `git command -flags options`. I (and many others) often find it easier to do some things in the terminal, and other things in various GUI programs, of which there are many. RStudio has some built in git functionality, as does Visual Studio Code. I also like GitKraken a lot, which is a paid app but which I've used for years with free student and teacher licences. For today, I'll show you a little of everything. 

## Before we begin...

Leaning Git can be challenging! It is a powerful tool, and can be very complex. It also has a lot of strange and unintuitive terminology. So, don't feel discouraged if you don't initially understand something, or run into some weird issues or error messages (we'll probably run into some today). Learning Git takes some work, but it is worth it (there's a reason that most of the R packages we use are developed on Git and Github)!

## Starting a Git/GitHub repositoy

Let's walk through making a Git/GitHub repository. There are a few ways to do this, but the easiest is to start the repo on Github. So, go to Github.com, sign in if you aren't already, and click "New". Give the repo a name. I recommend creating the repo with a README: it's good to have a README no matter what, and adding one now will automatically "initialize" the repository (which is a concept I'm going to skip over for now). Then, click create repository. 

At this stage, the repo exists and has been initialized, but the files are only on GitHub. We need to copy them down to our computer.

### Copy repository: `git clone`

In your terminal, navigate to the place where you want your repo to be saved. Copy the repo URL from GitHub via the green "Code" dropdown menu. Then, in your terminal, run:

`git clone https:://github.com/user/repo.git`

For public repositories, that should be it. For private repositories, you may have to enter your username and password or personal access token.

And that should do it! You should have on your computer some (probably empty) files that are the same as in your repo on GitHub. The big advantage of this approach, where you "clone" a repository FROM GitHub, is that it automatically links the git repo on your computer with the git repo on GitHub: when it comes time to sync the two (which we'll get to), you should already be set up. If you start your repository in other ways, you have to do a little setup (which we won't worry about for today). 

## A simple(?) workflow for working alone

Once you have a Git repository, working with it is conceptually straightforward (though not always straightforward in practice). You:

1. Make changes to your files, saving them to your computer as normal. 
2. Once you have made enough changes that you want to save the state of your files, tell git which files you want to save by "staging" them.
3. Then, tell git to save those changed by making a "commit" with a descriptive message. 
4. Periodically sync the files on your computer with the files on Github. 

One major conceptual point to keep in mind is that Git does not track your files constantly. It is more like a checkpoint system: when you tell it to, it saves/remembers the current state of files, and makes that a checkpoint you can return to in the future. These checkpoints are called "commits". You could also think of commits as snapshots, or versions. The main point to remember is that git does not save EVERY change you have ever made to a set of files: it only remembers the changes you tell it to remember.

Anyway, go make some changes to your files, and then we'll explore the next steps of the workflow.

### See the current state: `git status`

Tells you what files have changed or been added/deleted since the last commit, and whether any of those changes are "staged", which we will return to. This is a place where I think GUIs are useful: Rstudio, VSCode, and GitKraken all have nice displays of which files have changed since the 

### See specific change(s): `git diff`

`git status` only lists which files have changed since the last commit, it does not say HOW they have changed. The `git diff` command does that (it is similar to the unix `diff` that let's you compare files). Here again, I like GUIs better for this.

### "Stage" file(s): `git add`

Next, you tell git which changes/files you'd like to include in your next commit. In the command line, you use the `git add` command to stage changes. Again, the GUIs can be easier for this, as you can usually point and click. 

One thing to note: if you stage the changes in a file, and then make additional changes, you'll have to stage the file again for that second round of changes to be included in the commit. 

#### Sidenote: .gitignore

This is a text file where you list files for Git to ignore. Changes in these files won't be tracked, and the files won't be added to GitHub. Can use some wildcards and regular expressions to specify file names. 

Things to ignore:
* Large files: GitHub has a soft limit of 50MB for files, and a had limit of rejecting pushes >100MB. If repos get too large (i.e., a few GB) you may get warnings from GitHub.
* Local config files (e.g., .Rproj.user, bash/zsh histories and config settings)
* Locally installed software
* Files that can easily be regenerated (e.g., .html and .pdf reports from RMarkdown)
* Some types of binary files (.xlsx, .pdf). Sometimes it can be useful to track these, just to see if they have changed. But `git diff` will not be useful in seeing what has actually changed. 

### Save your staged changes: `git commit`

Once you've made the changes you want and staged them, you commit them to create your checkpoint/snapshot. You can use the `git commit` command, but again the GUIs have nicer options for writing nice commit messages. This is an important part of git: the commit messages provide information and context for you when you're going back and looking through your old code. So strive to write commit messages that are informative and concise. 

How often should you commit your changes? This is mostly up to you in terms of personal style, and the culture/expectations of the team you're working with. I generally try to keep commits focused on one issue or theme, so that the changes are clearer. Lots of small commits could be a little annoying, but I would err on the side of too many commits than too few. A big, cumbersome commit with hundreds of file changes can be hard to understand. 

### Sync your changes: `git push` and `git pull`

So, you've made a bunch of changes and had git remember them. At the moment, these changes are only present on your computer: Git isn't like Dropbox, it doesn't automatically sync file changes between your computer and github. You have to do that yourself, using two commands: `git push` sends changes from your computer to GitHub, and `git pull` retrieves changes from GitHub and puts them on your computer. The repo on github is often called a "remote". 

When you're working by yourself on a single computer, you should only need to push your changes. Once you start working across multiple computers, you need to be a little more careful. A general piece of advice is to run `git pull` often to make sure your local repo has all the latest changes from GitHub/the remote. To explain why consider this scenario:  

Say you have two clones of a repo: one on your laptop, and one on Grace. One day you edit the repo on your laptop, commit some changes, and push it to GitHub. Then, a few days later, you go to work on the same repo on Grace, and edit the same file, but without pulling the changes from GitHub first. When you go to push your commit to GitHub, it will be rejected: git doesn't know which version of the file to use: the changes you made on your laptop, or the changes you made on Grace. 
 
This is called a "merge conflict". We'll talk more about merge conflicts later, when we talk about collaborating. For now, I'll just say that an ounce of prevention is worth a pound of cure. When working across multiple computers, its best to always run `git pull` to check for and incorporate any recent changes from the remote branch before you start making more changes. 

Finally, a little note/tip for pushing: you don't need to push every commit immediately. In some cases, it is nice to wait a little bit: if you, say, discover a typo in your code or commit message you can fix that if your commit hasn't been pushed yet. But once a commit has been pushed to Github you can't easily edit it anymore. 

### Repeat

That basic workflow is enough to get you started. If you're working on one computer and working by yourself, that basic workflow of (1) making changes, (2) staging them, (3) committing them, and (4) pushing them to GitHub is all you need to share your code with others on GitHub. 

If time, experiment a little. Then, we'll move on to a couple more advanced conflicts that will allow us to collaboratively work on projects. 

## A simple(?) workflow for working together

### Concept: branches 


## Merge conflicts

## Further resources

https://happygitwithr.com/index.html

https://ohmygit.org

https://github.com/k88hudson/git-flight-rules

https://ohshitgit.com

https://r-pkgs.org/git.html
