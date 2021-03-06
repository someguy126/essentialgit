This work is licensed under the Creative Commons Attribution 3.0 Unported License. To view a copy of this license, visit http://creativecommons.org/licenses/by/3.0/ or send a letter to Creative Commons, 444 Castro Street, Suite 900, Mountain View, California, 94041, USA.

# Essential Git

## Introduction to This Guide

I learned most of what I know about Git from the book available online at <http://git-scm.com/book/>. I highly recommend it for anyone starting to learn Git.

Although I give that work high praise, I still wish to offer my own guide, which differs in certain respects that may make it more suitable for some people.

- Consideration of graphical clients: The book only considers the original Linux command-line client. This guide not only provides command-line syntax, but it also allows users of graphical clients to navigate them.
- Pragmatic organization: This guide introduces the most-needed concepts and tools first, and then less-used tools later.

This guide goes step by step. At each step new concepts are introduced, and later steps depend on the concepts introduced in previous steps. This means you will most likely have to read this guide in order.

## Introduction to Git and Version Control

If you are reading this guide, you probably already have some idea of what Git does. But maybe you just know the name and that it has something to do with software development, but you don't know what that "something" is.

(If you've already used another version control system such as CVS, Subversion ["SVN"], Mercurial, or Darcs, you can skip this section.)

In short, Git keeps track of files as they are changed over time. In other words, Git is a "version control system" (or a "revision control system").

The files that are being tracked can be of any sort: text, images, audio, video, and more. However, Git and other VCSes are usually used to manage source code for software, which is text. Git includes tools that only work for text files, and other tools that only work for source code specifically. Because of this, the examples in this guide will mostly be about software development. However, much of this guide also works for other types of file tracking.

Why is keeping track of changes important? For one thing, it lets you restore an old version if a change you made has a problem. It also lets multiple people work on the same files, often at the same time, and gracefully combine those changes into a final product.

## Getting Software

To start working with Git, you'll need to install some software. Git is a software program, but sometimes it's not used directly. It doesn't have its own graphical interface (with windows, buttons, and all that), so many graphical "client" programs have been developed to interact with the base program. Many of them are free, and the base program is free. A quick online search should give you what you need.

If you want, you can use the program directly, but it's a command-line program, and it was originally on Linux. So that means you need to know how to use a command-line, and specifically one on Unix/Linux. But you don't need to be working on a Linux system; there are Windows and Mac client programs that give you a Linux- (bash-) style command-line that works the same way for Git.

The command-line syntax is placed at the end of each section so that you can skip over it if you are using a graphical client. If you are using the command line, this also allows you to get a conceptual understanding before you start typing things into your terminal.

### Command-Line Basics for Git

(If you are using a graphical client, you can skip this subsection.)

The basic command-line syntax is:

    git [subcommand] [options]

Commands generally must be run when the terminal working directory is somewhere within the repository directory, otherwise Git will say `Not a Git repository`.

To get help on a subcommand:

    git help [subcommand]

Or:

    man git-[subcommand]

You can also use the help commands to get the full syntax, since this guide will only cover the most-used options.

The `-v` or `--verbose` option is available on almost all Git commands to display more information about what Git is doing. Place it right after the subcommand.

## Starting Work

After you've gotten the right software, it's time to begin working. First you need to create a repository. A repository (or "repo" for short) is a collection of all the files in a project and their histories.

A repository lies on your system. Instead of having a single repository that every contributor accesses, each contributor has a local repository where changes are made, and contributors share these changes among each other's repositories. This puts Git into a subclass of VCSes called DVCSes (distributed version control systems).

There are two ways to create a repository: initializing and cloning.

### Starting Fresh: Initialize ("Init", "New Repository")

Initializing basically means making a new, empty repository. Decide on a directory where you want to store it, and create a new repository in that directory. You can also create a new repository in a directory that already has files you're working on. That way it'll be easier to start tracking them. (It'll be easier, but they're not being tracked just yet.)

Graphical clients:

Graphical clients should have an option that reads "New" or "Init".

Command-line syntax:

    git init

### Adding to an Existing Project: Clone

Cloning means taking an existing repository and copying it to your system. Most likely it will be over a network or online (but, the Internet is a network, right?), but it could also be in another directory on your system.

If the repository is over a network, you should have a URL to it. Most likely it starts with either `http:`, `https:`, or `ssh:`. HTTPS may require authentication, such as a password. SSH requires some set-up, but that is beyond the scope of this guide.

If you clone a repository, keep in mind that the repository will actually be placed in a subdirectory of the directory you specify. The name of this subdirectory will be determined automatically based on the URL or the path, or it may be specified if your graphical client has this option.

Command-line syntax:

    git clone [repository] [directory]

This creates a repository within the current working directory. The `[repository]` option is the URL to a repository across as network, or the filesystem path to a local repository. The `[directory]` option is the name of the subdirectory; if it is not specified, the name will be determined automatically.

## Setting Up: Configuration

There are many ways to configure a repository, but there are two options you need to worry about: name and e-mail address.

Your name and e-mail address will appear alongside your changes. If you share your changes with other people (most likely you will), they will see this information.

Graphical clients make it easy for you to set up these options. You can set them for the individual repository, or for your whole user account on your computer. This is useful if you have a project for an employer as well as personal projects: You can use a company e-mail address for the repository for the company project, and a personal e-mail address for all other projects (perhaps in your user configuration).

Command-line syntax:

    git config [scope] [config-option] [value]

The `[scope]` option can be `--global` or `--system`, among others. If it is left out, the configuration will be set on the repository. The `--global` option indicates user configuration, and `--system` indicates system configuration (which may require administrator/superuser access).

The `[config-option]`'s you'll need are `user.name` and `user.email`.

If `[value]` has spaces, you'll have to put it in quotation marks for it to work.

## The Working Directory

A repository is a collection of files, and not just the files you work on for your project. But you might have some trouble finding those extra files for history and tracking.

If you navigate to the directory where you initialized a repository, it looks about the same. All your existing files are there, or there are no files as before. If you cloned a repository, there are the files of the project, but no indication of any history.

However, if you look at the hidden files in that directory, you will find a directory called `.git`. This directory is the core of the repository. It holds files for all the history and everything else Git needs to work. If you navigate into it, you'll find quite a few things you don't understand. But Git understands them, and you shouldn't try to work with these files directly.

The files you should work with are the files in the main directory of the repository. Appropriately enough, this is called the "working directory", or the "working area". This is where you make your changes. It's also where you compile your code (if it needs to be compiled) and run tests, just as if you didn't use Git at all. Git keeps everything it needs hidden from view in the ".git" directory.

## Looking at Changes: Status and Diff

Everything's set up, so now it's time to get working! After making some changes, it's a good idea to look at exactly what you did so you don't get lost. Git remembers where you were before you made any changes, so it can figure out what changes you made.

New files are usually called "untracked" because their histories are not yet being tracked by Git. Just think of it as meaning "new files." If you initialized a repository in a directory with existing files, those will show up as "untracked" because Git does not have any history of them yet.

If you've renamed a file, Git will usually show the old filename as "deleted" and the new filename as "untracked".

Graphical clients:

A graphical client will show you what changes you made. (You might have to select a "Refresh" or "Rescan" option to see all of them.)

The specific changes to each file might be shown in a separate area called "Diff" (short for "difference"). (Keep in mind that this generally only works for text files.) These changes may be presented in "unified diff" format:

    --- a/file.txt (The first two lines show the old and new filenames.)
    +++ b/file.txt (These will most likely be the same except for the "a" and "b".)
    @@ -10,4 +10,4 @@ (This line shows the range of lines that are affected in the old and new versions.)
     This is an unchanged line to show context.
     Unchanged lines begin with a space.
    -This line will be removed. It begins with a minus sign.
    +This line has been added. It begins with a plus sign.
     Changed lines will generally have the old line "removed" and the new line "added."

Command-line syntax:

    git status
    git diff [file]

The `git status` command shows a summary of the changes, such as which files were modified, added ("untracked"), or deleted.

The `git diff` command shows the specific changes to a file. It displays unified diff format by default.

## Marking Changes: Stage and Index

You've made some changes, but now you have to tell Git that you want to record those changes in the history. First you must tell Git exactly which changes you want to record. This is known as "staging" your changes. The "index" (also known as the "staging area") is a list of changes you've staged.

You can pick what to stage quite precisely. You can stage changes in files, deleted files, and added ("untracked") files. You can even stage only certain lines or sections in a file.

Graphical clients:

Graphical clients have various ways of allowing you to stage changes.

Command-line syntax:

    git add [file]
    git add -i
    git reset HEAD [file]
    git rm [file]

The `git add [file]` command stages all the changes in a single file. The `git add -i` command launches an interactive tool that allows you to do some of the more precise staging. (It might take some getting used to, though.)

The `git reset HEAD [file]` command unstages a file, while keeping the changes to it in the working directory.

The `git rm` command deletes a file. This is more "sophisticated" than deleting a file in the normal way because it also stages the deletion. (If you did delete it the normal way, though, just run `git rm [file]` to stage that deletion.)

## Recording Changes: Commit

You've decided what to changes to record, so now you can record those changes you've staged. This record is called a "commit", and the process of recording the changes is called "committing".

A commit represents the state of files in the project after a change. Each commit has any number of parent commits, which represent the state(s) of the files before the changes. Most commits have one parent. The first commit has no parents, since there is no state "before". Merge commits, which will be discussed later, have two (or possibly more) parents.

When you commit, you should explain your changes. As a matter of fact, Git won't under normal circumstances let you commit without including some sort of message.

The first line of the message should be a short summary of your changes. People going through the history will see this first line. Traditionally the second line is blank, and any subsequent lines will be a more detailed summary, if it is needed.

(Note: You might see an option in a graphical client called "Signoff". This adds a line that reads `Signed-off-by: Your Name <youremail@example.com>`. Even though your name and e-mail address are already recorded separately, the signoff becomes significant when commits are passed around between different people for approval before they make their way into the repository.)

Command-line syntax:

    git commit [options]

The `-a` or `--all` option automatically stages all changes (additions, modifications, and deletions) before committing. This means that you can commit without having to go through the staging process yourself, but it also means you might commit some things you didn't mean to commit. There are ways to recover from that, but still use with caution.

There are multiple ways to specify a commit message:

- The `-m [message]` or `--message=[message]` option uses the text in `[message]` as the commit message. This is recommended only for very small changes, since you can only fit a short message into this option. If you made more changes, consider another option to specify the message.
- The `-F [file]` or `--file=[file]` option takes the contents of the file in `[file]` as the commit message.
- The `-e` or `--edit` option opens a text editor for you to add a message.
- (The `-s` or `--signoff` option adds a sign-off line to the message.)
- The `--allow-empty-message` option tells Git to allow you to commit without a message. This is not recommended, but it's provided for automated programs that interact with the Git command-line program.

## Viewing History: Log and Show

Graphical clients usually have a visualizer that shows commits and their parents with lines. Clicking on a commit shows the message and the changes from the parent to that commit.

Each commit has an ID number, which is 40 hexadecimal digits (0-9 and a-f). This ID number is used to refer to the commit in a variety of situations. Don't worry, you don't need to use the whole thing. Git lets you use the first four digits, as long as the ID is unique from that point. If it's not unique in the first four, you'll need to give more. Most projects will only need up to seven digits.

(Technical Note: The ID number is actually the SHA-1 hash of the message, the state of the files, the name and e-mail address of the person, and so on.)

Command-line syntax:

    git log
    git show [ID]

The `git log` command shows the ID, author, date, and full message of each commit, in reverse chronological order. This command has various options to format the log to your liking, but these are beyond the scope of this guide.

The `git show` command shows the information for a single commit, including all the changes made in that commit.

## Branching and Checkout

A branch is a single line of work. Each branch has a name, which should reflect the kind of work being done on that branch.

If you've been working for a while, your commit history probably looks very linear, like this:

    1 - 2 - 3 - 4 - 5

In these diagrams, lines indicate parent relationships, and of course older commits are parents of newer ones. So, commit 4 is the parent of commit 5, commit 3 is the parent of commit 4, and so on. Commit 1 has no parent since it is the first in this repository.

This means you only have one line of work, that is, one branch. Every repository starts with one branch called `master`. A branch is really just a pointer to the latest commit in a line of work, and Git figures out the entire line by following the parents. The `master` branch looks like this.

    1 - 2 - 3 - 4 - 5 <- master

The `master` branch points to commit 5.

When you make a new commit, it will be based on the work that already exists on the `master` branch. That is, it will be based on the commit to which the `master` branch is pointing. This base has a special name, `HEAD`, which looks like this:

    1 - 2 - 3 - 4 - 5 <- master <- HEAD

Most of the time, `HEAD` points to a branch. When you make a new commit, Git moves HEAD and the branch to which it is pointing forward to the new commit. After another commit, `HEAD` and `master` look like this:

    1 - 2 - 3 - 4 - 5 - 6 <- master <- HEAD

`HEAD` still points to `master`, but `master` now points to commit 6.

But the beauty of Git and many other VCSes is that your work doesn't have to be this linear. You can have multiple lines of work. In software development, most work is done on different branches, and `master` is reserved for code that is tested, finalized, and released.

You can create a new branch that points to the same commit as `master`. Let's call it `test`. The result looks like this:

    1 - 2 - 3 - 4 - 5 - 6 <- master <- HEAD
                         \
                          <- test

Both `master` and `test` point to commit 6.

Notice that `HEAD` still points to `master`. When you make a new commit, `master` will be moved forward as usual, and `test` stays put. A new commit would result in this:

    1 - 2 - 3 - 4 - 5 - 6 - 7 <- master <- HEAD
                         \
                          <- test

Of course, this is probably not what you want; most likely, you want to work on `test`. That means moving `HEAD` to the `test` branch. To do that you'll need to "checkout" the branch. After checking out the test branch, it will look like this:

    1 - 2 - 3 - 4 - 5 - 6 - 7 <- master
                         \
                          <- test <- HEAD

Notice now that `HEAD` points to a different commit than before. This means that new work will be based on a different state of files. As a result, Git has to change the files in the working directory to match the state of this different commit. (For this reason Git will normally not allow you to switch branches if there are any uncommitted changes.)

So, the working directory matches commit 6, and the changes in commit 7 are removed from the working directory. A new commit will have commit 6 as a parent. This new commit looks like this:

    1 - 2 - 3 - 4 - 5 - 6 - 7 <- master
                         \
                          - - - 8 <- test <- HEAD

Graphical clients:

Graphical clients have various ways to create and checkout branches. Oftentimes there will be an option to checkout the branch immediately after creating it.

Command-line syntax:

    git branch
    git branch [newbranch] [startpoint]
    git checkout [branch]
    git checkout -b [newbranch] [startpoint]

The plain `git branch` command lists your branches. An asterisk (*) will be shown next to the current branch (that is, the branch to which HEAD is pointing).

The `git branch [newbranch] [startpoint]` command creates a new branch at the commit ID or branch given in the `[startpoint]` option, or at the same commit as HEAD if this option is not given.

The `git checkout [branch]` command checks out a given branch.

The `git checkout -b [branch] [startpoint]` command creates a new branch and then checks it out. It is short for `git branch [newbranch] [startpoint]` and then `git checkout [newbranch]`.

## Merging

Let's say you created and checked out a new branch at commit 7, called `stuff`:

    1 - 2 - 3 - 4 - 5 - 6 - 7 <- stuff <- HEAD
                         \   \
                          \   <- master
                           \
                            - 8 <- test

And then you do some work and commit on the `stuff` branch:

    1 - 2 - 3 - 4 - 5 - 6 - 7 - 9 - 10 <- stuff <- HEAD
                         \   \
                          \   <- master
                           \
                            - 8 <- test

After some consultation and testing, you decide that this work is ready to be moved into the "master" branch. This process is called "merging" the `stuff` branch into the `master` branch.

*Note*: It is recommended that you not have any uncommitted changes in your working directory before starting this process.

First you will have to checkout `master`, because that is the branch where you want to put new changes:

    1 - 2 - 3 - 4 - 5 - 6 - 7 - 9 - 10 <- stuff
                         \   \
                          \   <- master <- HEAD
                           \
                            - 8 <- test

Then you tell Git to merge `stuff`. Because `master` does not have any work that is not also in `stuff`, this merge is known as a "fast-forward". Git simply moves the `master` branch up to where the `stuff` branch is, like this:

    1 - 2 - 3 - 4 - 5 - 6 - 7 - 9 - 10 <- stuff
                         \           \
                          \           <- master <- HEAD
                           \
                            - 8 <- test

Now you can delete `stuff`. This is a common way of working with branches. After work in other branches is completed, those branches are merged into `master` and then deleted, because `master` now contains the work. The result looks like this:

    1 - 2 - 3 - 4 - 5 - 6 - 7 - 9 - 10 <- master <- HEAD
                         \
                          - - 8 <- test

But now let's say that you also want to move the work in `test` into `master`. This is not so simple because `master` has work that is not in `test`. Git deals with this by looking at the last commits in each branch (commit 10 and commit 8), as well as the "common ancestor" of the two branches (commit 6). It then combines the work in a new commit, called a merge commit with two parents. The result looks like this:

    1 - 2 - 3 - 4 - 5 - 6 - 7 - 9 - 10 - 11 <- master <- HEAD
                         \               /
                          - - 8 - - - - -
                               \
                                <- test

Notice that `test` is still pointing at commit 8. Only the branch you have checked out (`master` in this case) is moved forward. As a rule, you should checkout the branch where you want to include new changes. After a merge, the other branch stays where it is.

You might have noticed the phrasing before: "merge `test` into `master`". This phrasing is important. "Merge `B` into `A`" means checking out `A`. However, "merge `A` into `B`" means checking out `B`.

In this case you can also delete `test` because `master` now contains the changes on that branch. This is consistent with the practice of working on a branch, merging it, and then deleting it. Git can find the work on both branches by following both parents of the merge commit.

It is possible to merge more than two branches at the same time, in what is known as an "octopus merge" (though you probably won't have eight in total!). The merge commit in this case will have more than two parents:

    1 - 2 - 3 - 4 - 5 - 6 - 7 - 9 - 10 - - 12 <- master <- HEAD
                         \       \        / /
                          \       - - - 11 /
                           \              /
                            - 8 - - - - -/

But this is often not recommended because of conflicts, which will be discussed below. A better practice is to merge each branch individually.

Command-line syntax:

    git merge [options] [branch]

An octopus merge can be done by listing multiple branches separated by spaces in the `[branch]` option.

The `--no-commit` option does not make the merge commit. Git will combine the changes and stage them, but it will let you look around and possibly make changes before committing.

The `--abort` option aborts a merge if it is not yet committed. Git will restore the repository to where it was before.

The `-m [message]` option uses the text in `[message]` as the commit message. Normally Git creates an automatic message for you, but `-m` replaces the automatic message.

The `-e` or `--edit` option opens a text editor for you to add a commit message.

The `--ff-only` option tells Git not to peform the merge if it is not a fast-forward.

The `--no-ff` option creates a merge commit even if the merge is a fast-forward.

## Merge Conflicts

As shown above, fast-forward merges are very simple. Most merges that are not fast-forwards are also fairly simple. However, sometimes problems come up that might take some work and ingenuity to solve.

Let's look at the repository before the "test" branch was merged:

    1 - 2 - 3 - 4 - 5 - 6 - 7 - 9 - 10 <- master <- HEAD
                         \
                          - - 8 <- test

If different files were changed in commit 8 than in commits 7, 9, and 10, merging is easy: Git simply combines the new versions of the files. If the same files were changed, but in different places, Git combines those changes.

Problems come up if the same files were changed in the same places. This situation is called a "merge conflict" (or simply a "conflict").

A good way to see what happens is to take an example. Let's say there's a file called `README.txt`. After commit 6, the common ancestor of `master` and `test`, it looks like this:

    This project is to explore how to use Git,
    a version control system.

In the `test` branch (after commit 8), it was changed to this:

    This project is to explore how to use Git,
    a version control system (VCS).

In the `master` branch (after commits 7, 9, and 10), it was changed to this:

    This project is to explore how to use Git,
    a version control system, or revision control system.

The second line differs from the common ancestor, but in different ways. If you try to merge `test` into `master`, Git will stop the merge and alert you to a conflict. You will have to edit any conflicting files to a final product, and then stage them before you commit. (Files without conflicts will already be staged for you.) Although Git automatically lists the conflicting files in the commit message, you should also use the message to explain the conflicts and how you resolved them as well.

The simplest way to resolve a conflict, though it isn't really a resolution, is to abort the merge. Git will restore the repository to where it was before. Note that this means that any non-conflicting changes will not be merged, either.

Another way to resolve this is to take one version and get rid of the changes in the other. The version in the branch you have checked out (`master` in our example) is called "ours", and the version in the other branch (`test` in our example) is called "theirs". To resolve a conflict in this way, "checkout" the version you want and then stage it. (Note that this is a different use of "checkout".) Graphical clients may have a different way of resolving a conflict like this.

A "merge tool" can also help you resolve conflicts by showing the conflicting sections and letting you decide on the final product. But if you don't have one, you'll have to manually edit the file in the working directory. If you open it, it will look something like this:

    This project is to explore how to use Git,
    <<<<<<< HEAD:README.txt
    a version control system (VCS).
    =======
    a version control system, or revision control system.
    >>>>>>> test:README.txt

Conflicting sections are marked with `<<<<<<<`, `=======`, and `>>>>>>>`. `<<<<<<<` marks the beginning of the section, `=======` separates the two versions, and `>>>>>>>` marks the end of the section.

You can look at each version and put them together. (Remember to remove those markers when you're done!) In this example, a final result might be:

    This project is to explore how to use Git,
    a version control system (VCS),
    also known as a revision control system.

You can then stage this file and commit.

There are two cases of merge conflicts that are perhaps not obvious.

The first case concerns files that are not plain text (these are also known as "binary files"). If Git detects a conflict in a binary file, it will not add any conflict markers. Instead, it will keep the version as it was at `HEAD`. You can use the ours/theirs method, or run a merge tool. Some merge tools create files such as `[conflictingfile].HEAD` and `[conflictingfile].REMOTE`, which contain the different versions, in your working directory to help you combine changes.

The second case concerns octopus merges. Trying to imagine what the conflict markers would look like is difficult. Actually, this is why octopus merges are not recommended. In the case of a conflict, Git will abort the merge entirely (and rather sternly tell you that you `should not be doing an Octopus`).

Command-line syntax:

    git merge --abort
    git mergetool

The `git merge --abort` command aborts a conflicting merge and resets the repository to its state before the merge.

The `git mergetool` command starts a merge tool if one is installed.

## Setting Up Other Repositories: Remotes

Chances are that you're not working alone. Remember that every contributor has a repository. So in order to transfer your changes to others, you will have to work with "remote repositories" (or simply "remotes"). A remote may be another repository on your system, or (more likely) a repository available through a network such as the Internet.

Your repository has records of these remote repositories that allow your repository to interact with them. These records include the URL or path to the repository, a name used to refer to it within your own repository, as well as "remote-tracking branches," discussed below.

Note: If you're working with a repository over a network, you may need some sort of authentication, perhaps a password or SSH. This authentication is beyond the scope of this guide.

The first step in working with remotes is to "add" it to your repository. This means creating a record of the URL or path, as well as a name. The name should be descriptive of its source, such as "company", "sourceforge", or "github". (SourceForge and GitHub are popular hosting sites for Git repositories.)

(If you cloned from a repository, Git already added a remote called `origin`, which refers to the repository from which you cloned. Git also already did a lot of the setup, which will be noted in each subsection.)

Command-line syntax:

    git remote
    git remote add [name] [repository]
    git remote rm [name]
    git remote set-url [name] [repository]

The plain `git remote` command lists the remotes currently recorded.

The `git remote add` command adds a new remote. The `[name]` option is the name of the remote used in your repository, and `[repository]` is the URL or path to the actual repository.

The `git remote rm` command removes a remote; `[name]` is the name of the remote to remove.

The `git remote set-url` command reassigns a remote to a new URL or path. (The command is called `set-url`, but a filesystem path works, too.)

### Getting Commits from Remotes: Fetch

The next step is getting any commits that are already in the repository. This process is known as "fetching".

Some interesting things happen when you fetch.

Suppose this is a remote repository that you've called `ourproject`:

    1 - 2 - 3 - 4 - 5 - 6 - 7 - 9 - 10 - 11 <- master <- HEAD
                         \               /
                          - - 8 - - - - -
                               \
                                <- test

And this is your repository:

    1 - 2 - 3 - 4 - 5 <- master <- HEAD

If these repositories are for the same project, commits 1-5 will be the same.

After you fetch, your repository will look like this:

    1 - 2 - 3 - 4 - 5 - 6 - 7 - 9 - 10 - 11 <- ourproject/master
                     \   \               /
                      \   - - 8 - - - - -
                       \       \
                        \       <- ourproject/test
                         \
                          <- master <- HEAD

Git combined the commits from the remote with the ones in your repository. Git figured out that the commits in your repository are the same as the ones in the remote, so Git combined the commits it fetched with the commits you already have.

Notice that Git created new branches in your repository that start with `ourproject/`. These are called "remote-tracking branches". These reflect where the branches are on the remote when you last fetched from it. Git treats these branches differently; you can't checkout these branches in the normal way, and you can't delete them in the normal way either.

(If you cloned from a repository, Git has already fetched commits from it and created the remote-tracking branches starting with `origin/`.)

Most likely, the remote includes commits from other contributors, so you should include them in your own work. Git won't let you put your work into the remote if it doesn't include other contributors' work as well. To include their work, merge "ourproject/master" into "master". Afterwards, your repository will look like this:

    1 - 2 - 3 - 4 - 5 - 6 - 7 - 9 - 10 - 11 <- ourproject/master
                         \               / \
                          - - 8 - - - - -   <- master <- HEAD
                               \
                                <- ourproject/test

Notice how the merge was a fast-forward, because your `master` has no work that is not also in `ourproject/master`. When you merge remote-tracking branches, they're just like any other merges. (Merge commits can be created, and conflicts can come up.)

Oftentimes branches are removed from the remote because they've been merged. Git doesn't automatically remove the remote-tracking branches from your repository, though. To do this, you should be able to specify an option to "prune" the remote-tracking branches when you fetch.

Command-line syntax:

    git fetch [-p] [remote] [refspec]
    git remote prune [remote]

The `[remote]` option is the name of the remote, and `[refspec]` is the branches to fetch. It can get complex, but the simple case is just a list of branches separated by spaces. If `[refspec]` is omitted, Git will fetch all branches.

The optional `-p` option (shown in brackets in the syntax) tells Git to prune the remote-tracking branches whose branches no longer exist in the remote repository. You can also run `git remote prune` to do this without fetching new commits.

## Getting and Merging Commits from Remotes: Pull and Tracking

"Pulling" is an automatic process that fetches and then merges in one step. It's a bit strange: Git fetches the commits from all branches (and updates the corresponding remote-tracking branches), but it only merges the branch to which `HEAD` points. The appropriate remote-tracking branch is merged in.

Wait, "appropriate" remote-tracking branch? In our example we merged `ourproject/master` into `master`. They have the same name (at the end, at least), so it makes sense. But what if you had two remotes, both with `master` branches? Then you'd have `ourproject/master` and also, say, `other/master`. Which would be merged?

If your repository is not set up, the answer is neither. As a matter of fact, Git won't automatically merge `ourproject/master` even if it is the only remote-tracking branch with the name `master`.

To set this up, you need to set your branch to "track" this remote-tracking branch. Yes, there are two uses of the term "track". Your branches track remote-tracking branches, and the remote-tracking branches track the actual branches on the remote.

This is actually a good way to think about it:

    your branches --(track)--> remote-tracking branches --(track)--> branches on remote

When you pull, the commits move in the opposite direction, like this:

    your branches <--(merge into)-- remote-tracking branches <--(merge into)-- branches on remote

Setting up your branches to track remote-tracking branches automates the pulling process, and it also helps you work with multiple remotes. Remember `ourproject/master` and `other/master`? You could set your `master` branch to track `ourproject/master` and another branch called `othermaster` to track `other/master`. That way you can pull from both remotes and make sure the merge process goes the way you want.

(If you cloned from a repository, Git already set up your branches to track all the remote-tracking branches from `origin`. You can change this tracking if you'd like.)

Command-line syntax:

    git pull [remote] [refspec]
    git pull --all [refspec]
    git branch {-u [upstream] | --set-upstream-to=[upstream]} [branch]

The `git pull` command pulls from a remote, that is, fetches all branches and merges certain branches into the branch to which `HEAD` points. The `[remote]` option is the name of the remote. Use `--all` instead of the name of a remote to fetch from all remotes. The `[refspec]` option is the same as for fetching. That is, it can get complex, but the simple case is a list of branches separated by spaces. With this, all of the listed branches are merged into the current branch (the branch to which `HEAD` points), regardless of tracking. (This may result in an "octopus.") If `[refspec]` is omitted, Git will fetch all branches from the given remote and merge the tracked branch into the branch to which `HEAD` points.

The `git branch` command with the options shown sets up tracking. It tells the branch named in `[branch]` (one of your branches) to track `[upstream]` (a remote-tracking branch). The `-u [upstream]` and `--set-upstream-to=[upstream]` options mean the same thing.

## Putting Changes into Remotes: Push

After you've done your work, it's time to put your work into the remote repository. This is called "pushing" to the repository.

Your branches tracking the remote-tracking branches also determine where your work will go when you push:

    your branches --(merge into)--> remote-tracking branches --(merge into)--> branches on remote

The merges in the diagram must be fast-forwards. Your branches must include all the work in the remote repository. If they don't, Git will refuse to push under normal circumstances. Even if you've merged the remote-tracking branches, the actual branches may have new work. So you might have to pull (fetch and merge) before you push.

Command-line syntax:

    git push [remote] [refspec]

The `[remote]` option is the name of the remote, and `[refspec]` is the branches to push. It can get complex, but the simple case is just a list of branches separated by spaces. If `[refspec]` is omitted, Git will push all branches that have corresponding branches with the same name on the given remote. (This means that, if you created a new branch and want to push it to a remote where it doesn't exist yet, you will have to specify the name.)
