---
layout: post
title:  "GIT IT DONE"
date:   2017-03-02 02:21:13 +0000
---

After doing the [Flatiron Students Present on GIT](https://docs.google.com/presentation/d/1oUI4ZFEgfg2ZE060B9UGkjiLCNKLwddsruAEHYWbps8/edit?usp=sharing) this past Tuesday, I decided to go deeper into Git and explore beyond the basics.

Here is an review of what I covered in the presentation:

![](http://i.imgur.com/yZpy24H.png)

Lets talk about using Git in the real world. Git is very useful just as a version control, but in reality, Git is generally used along side a collaboration hub like Github to work with other people. There are many Git Workflow strategies enterprises will implement to manage permission of changes to code across organizations. A really good resource covering the basics is [Atlassian's Tutorial on Comparing Workflows](https://www.atlassian.com/git/tutorials/comparing-workflows). I'd like to discuss 1 common workflow from Atlassian's Tutorial and how collaborators on projects can work together to 'merge' code. 

## Centralized Workflow

In a centralized workflow, one person will intialize a repository and everyone will be added as collaborators to that repository. Everyone the clones from the central repository, using `git clone <ssh>`

1. Lets say one developer finishes their story first. They can `git add` and `git commit` their changes to their local cloned repo master branch
2. They can then use `git push origin master` to push their code up to the centralized remote repository. In this case "origin" is the name of the remote repo and "master" is the name of the branch in their local branch they are pushing from.
3. Now lets say another developer is ready to push their local commits to the central repository. When they do `git push orgin master` they will receive a message from Git in their terminal saying that their permission is denied becuase **the origin/master branch is ahead of their local master branch**.
4. In order to get their master branch up to date, they will need to pull down the most current version of the central repository's master branch, using a command like `git pull origin master`, which will checkout and merge the remote repo branch "orgin/master" into the branch we are currently checked out in.
5. Once the local master is up to date with the remote master and no merge conflicts exist, the second developer can use the command `git push origin master` like the first developer to push their changes up to the central remote repository
6. Any any additonal developers will need to do the same thing. In summary:


* Commit your changes locally: `Git add .` and `git commit`
* Pull the remote master: `git pull origin master`
* Manage merge conflicts
* Push your up to master to the remote master: `git pull origin master`


We could be done here, but in the real world there is one important distinction between the process I just laid out and what actually happens. When we do a pull, we are fetching the remote master branch and then merging it into our current local branch. This would look something like:

![](http://i.imgur.com/U8dhqBJ.png)


This creates a none linear commit log with a 3 way merge, which means that anytime someone pulls down the master branch they will have this merge commit and any other merge commits along the way, obfuscating the changes in code over time. Rather than have a lot of non-linear commits, it is better to rebase first:

![](http://i.imgur.com/qEtgCSc.png)

And then do a `git checkout master` and `git merge origin master`

![](http://i.imgur.com/Jp1TDXb.png)

This will create a linear commit history by first rebasing your local master branch and then merging it to the master. Rebasing takes endpoints of branch and replays it onto another branch in the order they were introduced, where as merging takes the endpoints of branches and mergest them together. Other benefits of rebasing are that you only resolving merge conflicts one commit at a time and it is very easy to abort or undo.

In summary, to use the centralized workflow, developers need to:

1. `git clone <ssh of centralized repo>`
2. `git add .` and `git commit -m`
3. `git pull --rebase origin master` (doing a rebase and then a merge)
4. `git push origin master`

Sources of pictures: [GIT-SCM](https://git-scm.com/book/en/v2/Git-Branching-Rebasing)




 




