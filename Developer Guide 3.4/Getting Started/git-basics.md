# Git Basics

> Install and configure Git and then learn some basic commands to get you started.
 
## Installing Git

Git can be installed on most operating systems including Linux, Windows and MacOS. Instructions for installing Git can be found [here](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).

## GitHub Account

As TAO's codebase resides on GitHub you will need to go to their site and [create](https://github.com/join) an account before proceeding. This will allow you to collaborate with the TAO development team, fork the TAO project and send pull requests. It is also recommended you configure an SSH key in your account.

## Configuring Git

Set username for Git, this should match your GitHub account.

```
git config --global user.name "<username>"
```

Set your email address for Git, this should match your GitHub account.


```
git config --global user.email "<email address>"
```

## Common Git Commands

The following are some of the most common Git commands.

Create a local version of a remote repository

```
git clone <repo>
```

List all branches in the repo

```
git branch
```

These are 2 different ways you can create a new branch

```
git branch <new branch>
git checkuot -b <new branch>
````

You can also checkout an existing branch

```
git checkout <branch>
```

List new or modified files that are not committed

```
git status
```

Snapshot a file in preparation for committing

```
git add <file>
```

To commit your changes and br prompted for a title and comments

```
git commit
```

To upload your commits to the remote repository

```
git push <alias> <branch>

```

And the last basic command, is to pull down newer changes from the remote repository

```
git pull
```

For a complete list of Git commands, use *man git* or check out the offical [docs](https://git-scm.com/docs).