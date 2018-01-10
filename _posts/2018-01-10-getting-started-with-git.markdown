---
layout: post
comments: true
title: Getting started with git
date: 2018-01-10
---

   * [Some background information](#some-background-information)
   * [Basic git commands](#basic-git-commands)
	 * [Create a repository from the current working directory](#create-a-repository-from-the-current-wo
rking-directory)
	 * [Clone a existing repository from GitHub](#clone-a-existing-repository-from-github)
	 * [Commit changes and push to GitHub repositiory](#commit-changes-and-push-to-github-repositiory)

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

*\<to be continue>*
