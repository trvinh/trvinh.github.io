---
layout: post
comments: true
title: Getting started with git and GitHub
date: 2018-01-10
---

Table of Contents
=================
   * [Some background information](#some-background-information)
   * [Basic git commands](#basic-git-commands)
      * [Create a repository from the current working directory](#create-a-repository-from-the-current-working-directory)
      * [Clone a existing repository from GitHub](#clone-a-existing-repository-from-github)
      * [Commit changes and push to GitHub repositiory](#commit-changes-and-push-to-github-repositiory)
   * [Git branching](#git-branching)

# Some background information
<ol>
<li>The importance of <a href="https://git-scm.com/book/en/v1/Getting-Started-About-Version-Control" target="_blank">version control</a>.</li>
<li>What is <a href="https://git-scm.com/book/en/v1/Getting-Started-Git-Basics" target="_blank">git</a>?</li>
<li>How to <a href="https://git-scm.com/book/en/v1/Getting-Started-Installing-Git" target="_blank">install git</a>?</li>
<li>What is <a href="https://github.com" target="_blank">GitHub</a>?</li>
</ol>

# Basic git commands
*Summarized from <a href="https://git-scm.com/book/en/v1/Git-Basics-Getting-a-Git-Repository" target="_blank">this page</a>.*

### Create a repository from the current working directory
In the current folder, type
`$ git init`. This creates *.git* folder which contains all necessary files for this repository.

### Clone a existing repository from GitHub
`$ git clone https://github.com/bionf/phyloprofile` where https://github.com/bionf/phyloprofile is the URL of the existing repository in GitHub.

### Commit changes and push to GitHub repositiory
After making some changes (edit files, create and remove files), you can check the status of the current repository by typing `$ git status`.

A list of untracked files will be listed. In order to start version-controlling those files, use `$ git add *` (or add only files / file types that you would like to track).

Then you can commit the changes you have made with `$ git commit -m "type some message here"` and update the corresponding GitHub repository by typing `$ git push`.

# Git branching
[Git branching](https://git-scm.com/book/en/v1/Git-Branching-What-a-Branch-Is) is a "killer feature" in git. You can create another branch of the repository to work without affecting to the main branch (*master branch*), e.g. make some new features, change some function for your own purpose, etc.

To create a new branch called new_function:

`git checkout new_function`

You have made a new pointer at the current commit you are currently on. Now you are working on the *"new_branch"* branch. Making any changes here will not effect to the main *"master"* branch. To commit and push changes to this branch follow the  [instruction above](#commit-changes-and-push-to-github-repositiory).

To change back to the *master* branch (or go to any other branches), use the command:

`git checkout "branch_name"` (e.g. `git checkout master`)

The command `git checkout` without any branch's name will tell which branch you are currently on.

After successfully created new functions for your tool, you can merge the changes in branch *"new_function"* to *"master"* by doing:

```
git checkout master
git merge new_function
```

After everything have been done, *"new_function"* branch will be no longer needed, we can now delete it:

`git branch -d new_function`

*\<to be continued>*
