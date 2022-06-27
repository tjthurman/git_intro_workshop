# Git and GitHub

## What is Git?

A free, open-source *version control system* for tracking changes to code (and other files). Think of it like track changes in Word, or the version history in Google Docs. It keeps a record of changes you make to your files. It allows you to see the history of changes you've made, compare file versions, and to restore previous versions of your code if you introduce new bugs. Though useful by itself, it really shines when you combine it with GitHub.

## What is Github?

GitHub is a website where you can store Git repositories, share them with others, and collaborate on code. It's a little like Dropbox, Box, or Google Drive, but specialized for working with Git repos. It requires you to create an account (which is free), though they also have some paid plans. 

## Interacting with Git/Github

You can work with git in a number of ways: it is a command line program, so you can do everything from the terminal. I (and many others) often find it easier to do some things in the terminal, and other things in various GUI programs, of which there are many. RStudio has some built in git functionality, as does Visual Studio Code. I also like GitKraken a lot, which is a paid app but which I've used for years with free student and teacher licences. For today, I'll show you a little of everything. 

## Before we begin...

Leaning Git can be challenging! It is a powerful tool, and can be very complex. It also has a lot of strange and unintuitive terminology. So, don't feel discouraged if you don't initially understand something, or run into some weird issues or error messages (we'll probably run into some today!). Learning Git takes some work, but it is worth it (there's a reason that most of the R packages we use are developed on Git and Github)!

## Starting a Git/GitHub repositoy

Let's walk through making a Git/GitHub repository. A

### git init





## A simple(?) workflow for working alone

Once you have a Git repository, working with it is conceptually straightforward (though not always straightforward in practice). You:

1. 


### See the current state `git status`

### See change(s) `git diff`


### "Stage" file(s) `git add`

Only adds the current state of files

#### Sidenote: .gitignore

Large files. Things that can easily be regenerated. binary files.  

### Save your changes: `git commit`

### Sync your changes: `git push` and `git pull`


A good habit is to pull frequently. 

## A simple(?) workflow for working together



## Further resources
