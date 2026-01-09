---
title: "Git"
weight: 6
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
# bookIcon: ''
---

# Git how-to
[Git](https://en.wikipedia.org/wiki/Git) is a software version control system to keep track of changes to your software development project. Git allows you to make and keep track of small updates ("commits") as you develop your software. These commits _should_ be safe checkpoints with descriptions of what changed such that you can revert to if your latest changes immobilize or destroy your robot. As such, they are also a great way to document your progress. Git also allows you to prototype with a separate copy of the software without affecting the main copy ("branch" out). Therefore, it is also a great tool to collaborate with others as you can have separate copies of the software and develop independently.

## Installation
Git is already installed on the Pi. You should also install Git on your own computer to write software on your computer without the Pi.

### OS X
OS X should have Git installed by default. 

### Linux
Linux should also have Git installed by default. If not, checkout https://git-scm.com/install/linux and install with your appropriate distro.

### Windows
For Windows, download and run the "Standalone Installer" from https://git-scm.com/install/windows.

### Git version
Try this command to print out the version of Git on your system.
```shell
git -v
```

## Accessing MIT Git
MIT hosts a Git server accessible to all MIT students at https://github.mit.edu. Your team software should also be hosted at https://github.mit.edu/maslab-2026. Check that you are able to access your team's software repository there. If not, please contact a course staff to get access permission.

### Setting up SSH on your personal computer
Git also uses SSH to connect to Git servers. Follow [this guide](https://docs.github.com/en/enterprise-server@3.4/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account) to set up and connect an SSH to your MIT GitHub account. You DO NOT have to do things with GitHub CLI (commands starting with `gh`).

> [!IMPORTANT]
> Make sure to add that at https://github.mit.edu, where you have to sign in with your Kerberos, **NOT** the public https://github.com. 

### Setting up SSH on the Pi
In order to get read/write access to your repository from the Pi, you can add a repository-specific "Deploy Key." This lets you access a repository from the device without having to add a key for your Pi on any individual team member's account.

To add a deploy key to your Pi, follow the steps in "Set up deploy keys" in [this guide](https://docs.github.com/en/enterprise-server@3.4/authentication/connecting-to-github-with-ssh/managing-deploy-keys#set-up-deploy-keys). In these instructions, "server" refers to the Pi. Your team repository should have been created for you, and can be found at `github.mit.edu/maslab-2026/team-<number>`.

## Using Git
### Configure user
To ~~blame~~ know who make code changes, Git needs to be configured with a default name and email address to be linked to each commit. To do that, run these command on your computer and replace `<your name>` and `<your email>` appropriately:

```shell
git config --global user.name "<your name>"
git config --global user.email "<your email>"
```

As for the Pi, feel free to use `teamX` and `teamX@teamXpi` for name and email, replacing `X` with your team number.

### Tutorials
Now that you have Git installed and set up, you can follow these tutorials to try using Git on your own computer.

1. Easy way: https://learngitbranching.js.org/. Do "Main -> Introduction Sequence" and "Remote -> Push & Pull -- Git Remotes!"
1. h4rdc0r3 way: https://missing.csail.mit.edu/2020/version-control/.
1. Git in VSCode: https://code.visualstudio.com/docs/sourcecontrol/quickstart. For this tutorial, you can clone your team repository at `github.mit.edu/maslab-2026/team-<number>`. In addition, follow up with [branches](https://code.visualstudio.com/docs/sourcecontrol/branches-worktrees#_working-with-branches) and [resolve merge conflicts](https://code.visualstudio.com/docs/sourcecontrol/merge-conflicts).

## What's next
Now that Git is set up on your computer and the Pi, make sure your teammates who would want to develop the robot software also has it set up. Then, clone your team's repository to your computer and the Pi and start developing!
