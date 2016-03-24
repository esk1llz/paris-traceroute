

# Configure git #

1) Configure the client:

```
git config --global user.name "Firstname Lastname"
git config --global user.email "firstname.lastname@foo.com"
git config --global core.autocrlf false
git config --global core.filemode false
git config --global core.editor vim
git config --global push.default current
git config --global color.ui true
```

2) Connect to gmail or any google application (the following link will then display additional information). Configure your ~/.netrc as explained in this link:
https://code.google.com/p/paris-traceroute/source/checkout

At least, this file should look like this:

```
machine code.google.com login my.account@gmail.com password p4ssw0rd_G3n3r4t3d_By_G00gle
```

# Using git with a given branch #
## Checkout ##

To get the sources, we refer to [this page](Installation.md).

```
git clone https://code.google.com/p/paris-traceroute.libparistraceroute/
```

So far, only the "master" branch appears. The other remote branches are currently hidden:

```
git branch
git branch -a
```

If you want to work on an existing branch which is not "master", (let say "devel") you should create a local branch pointing to the corresponding remote branch.

```
git checkout -b devel origin/devel
git branch
```

## Update ##

Everytime you need the last version of the sources.

```
git fetch
git merge origin/myBranch
```

This should solve the most of the conflicts. Avoid "git pull".

If someone has pushed some updates since your last commit on this branch, push your merge by running:

```
git push
```

## Add ##

If you're adding new file(s) to the library, update "Makefile.am" as explained [here](Makefile.md). Then go to the "Commit" section.

## Commit ##

Everytime you want push your updates to the other developers. You will first to add every added and modified files. Then commit & push...

```
git add myModifiedFile1 ... myAddedFile1 ...
git commit
git push
```

Suppose you're working with a branch named "myBranch". To select this branch :

## Revert ##

List revisions related to a given file foo.c

```
git log foo.c
```

Suppose that we want to restore foo.c using the "abcde" revision:

```
git checkout abcde foo.c
```

## Show updates of a given revision ##

Show the updates provided by the "abcde" revision:

```
git show abcde
```

You can have a better rendering by using kompare:

```
git show abcde > /tmp/diff
kompare /tmp/diff &
```

# How to manage branches #

## Create a new branch ##

Create the branch on your local machine :

```
git branch newBranch
```

Push the branch on git :

```
git push origin newBranch
```

Switch to your new branch :

```
git checkout newBranch
```

## Push a local branch to the git server ##

Suppose that "myBranch" is a local branch.

```
git push -u origin myBranch
```

To get this newly created branch from another PC and track this remote branch, run:

```
git fetch origin
git checkout --track origin/mando
```

## Change the current branch ##

Suppose you want to use "myBranch":

```
git branch
git checkout myBranch
git branch
```

## Compare branches ##

  * List the modified files:

```
git diff --name-status master..origin/myBranch
```

You may also see how many lines have been altered by using --stat        instead of --name-status

  * Get the modifications:

```
git diff master..origin/myBranch [file]
```

## Merge branches ##

1) Merge "myBranch" into "master":

```
git checkout master
git merge myBranch
```

2) You can now delete "myBranch" (see next section).

## Delete a branch ##


  * If "myBranch" is a local branch:

```
git branch -d myBranch
```

  * If "myBranch" is a remote branch: two possible syntaxes:

```
git push origin :myBranch
git push origin --delete myBranch
```

# Patches #

  * Patches may be built using git format-patch.

  * The contents of such a patch looks like a "git diff" command (and includes email address, commit message and so on). To apply such a patch named foo.diff:

```
git apply --ignore-space-change --ignore-whitespace foo.diff
```

# Fix invalid commit messages #

(TO IMPROVE)

To update one or several commits message among the 5 lasts commits, run:
```
git rebase -i HEAD~5
```

Change "pick" into the appropriate keyword (for instance "reword") to trigger interactive modifications on the appropriate commits. Save and quit this file. Your editor will pop allowing to rephrase each commit message.