#computer_science #github #git 

[[Git Backup]]

# Git Cheat Sheet

## Introduction

This page provides a quick reference for common `git` commands. For a more detailed introduction to `git`, see the [introductory page](gitintro) and references therein.

## Common Commands

### Clone a repository

Cloning is the process of downloading a copy of a repository from a remote server to your local machine. This is the first step in working with a repository that you do not already have on your computer

To clone a repository from a remote server, use the `git clone` command followed by the URL of the repository. For example, to clone the GitHub repository at `https://github.com/ese-msc/introduction-to-python`, use the following command:

```bash
git clone https://github.com/ese-msc/introduction-to-python
```

Depending on the repository, you may need to authenticate with a username and password, or an SSH key.

You can also clone from a repository on a different server, such as GitLab or Bitbucket, or even a local repository by replacing the URL with the appropriate address. You can also clone a repository to a specific directory by adding the directory name after the URL.

```bash
git clone https://github.com/ese-msc/introduction-to-python my-python-repo
```

### Check the status of a repository

The `git status` command is used to display the current status of a repository. This includes information about which files have been modified, which files are staged for commit, and which branch you are currently on.

To check the status of a repository/branch, use the `git status` command. For example, to check the status of the current repository, use the following command:

```bash
git status
```

### Switch to a branch

Branches are used in `git` to allow multiple versions of a repository to be developed in parallel. The `git branch` command can be used to create, list, and delete branches, while the `git checkout` command is used to switch between branches.

To switch to a branch, use the `git checkout` command followed by the name of the branch. For example, to switch to the existing `develop` branch, use the following command:

```bash
git checkout develop
```

If the branch does not exist, you will receive an error message. You can ensure that a new branch will be created if it doesn't exist with the `git checkout -b` command followed by the name of the new branch.

```bash
git checkout -b new-feature
```

### Pull changes from a remote repository

When working with a repository that is shared with others, it is important to keep your local copy up to date with the changes made by others. The `git pull` command is used to fetch and merge changes from a remote repository into your local copy.

To pull changes from a remote repository, use the `git pull` command. For example, to pull changes from the `origin` remote repository into the current branch, use the following command:

```bash
git pull origin
```

This will fetch any new commits from the remote repository and merge them into your local copy. If there are conflicts between your changes and the changes from the remote repository, you will need to resolve them before the changes can be properly merged.

### Stage and commit file changes

Unlike some other version control systems, git uses a two-step process to commit changes to a repository. First, changes are "staged" using the `git add` command, and then they are committed using the `git commit` command.

To stage changes to a file, use the `git add` command followed by the name of the file. For example, to stage changes to the file `example.py`, use the following command:

```bash
git add example.py
```

The `git add` command can also be used to stage all changes in the repository by using the `.` wildcard. For example, to stage all changes in the repository, use the following command:

```bash
git add .
```

This is **not** recommended for large or shared repositories, as it can lead to accidentally staging changes that you did not intend to commit. Instead, it is better to stage changes to individual files or directories.

Once changes have been staged, they can be committed using the `git commit` command. For example, to commit the staged changes with the message "Add new feature", use the following command:

```bash
git commit -m "Add new feature"
```

If you forget to include the `-m` flag, `git` will open a text editor for you to enter a commit message. This can be configured to use your preferred text editor, such as `vim` or `nano`. It's a good idea to set this up before you start using `git` regularly, since the default editor (`vim`) can be difficult to use if you're not familiar with it.

### Push changes to a remote repository

Once changes have been committed to your local repository, they can be pushed to a remote repository to share them with others. The `git push` command is used to push changes from your local repository to a remote repository.

To push changes to a remote repository, use the `git push` command followed by the name of the remote repository and the branch you want to push. For example, to push changes to the `origin` remote repository in the `develop` branch, use the following command:

```bash
git push origin develop
```


## Additional Commands

### Temporarily store changes

The `git stash` command is used to temporarily store changes that are not quite ready to be formally committed. This can be useful if you need to switch branches or work on a different task without committing your changes.

To stash changes, use the `git stash` command. For example, to stash all changes in the repository, use the following command:

```bash
git stash
```

To revert back the last stash, use the `git stash pop` command. For example, to revert the last stash, use the following command:

```bash
git stash pop
```
