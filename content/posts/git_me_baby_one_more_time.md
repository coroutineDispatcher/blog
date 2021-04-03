---
title: "Git me baby one more time"
date: 2021-04-03T12:15:57+02:00
draft: false
---

Long time no see. Nothing new on Android from my side this days. I have mostly been focused in learning some new tech stack and some automation. But today I would like to talk about some basic git commands/concepts and situations.

## Well, let's jump to some ancient debate: Rebase vs Merge

For any reader who doesn't know the difference, I am trying to illustrate it in the below diagram. Let's consider these 2 branches:

&nbsp;

![Branch_A_B_Illustration](/images/git_rebase_vs_merge.png)

&nbsp;

### git merge

Let's start with merge first. From the official git documentation:

{{< admonition type=info title="Name">}}
git-merge - Join two or more development histories together
{{</ admonition >}}

{{< admonition type=info title="Description">}}
Incorporates changes from the named commits (since the time their histories diverged from the current branch) into the current branch.
{{</ admonition >}}

In other words, you don't care what happened to previous commits of branch B. You just take the last one, and put it together with the current one:

&nbsp;

![git_merge](/images/git_merge.png)

&nbsp;

Think of the merging point as a huge commit that has basically everything, your current work, including the one you merged. If you think further, that's what happens after merging a Pull Request (or a Merge Request).

For more information, you can refer to the official [documentation](https://git-scm.com/docs/git-merge).

### git rebase

Let's try the same scenario, but now talking about rebase. From the official docs:

{{< admonition type=info title="Name">}}
git-rebase - Reapply commits on top of another base tip
{{</ admonition >}}

{{< admonition type=info title="Description">}}
If \<branch\> is specified, git rebase will perform an automatic *git switch* \<branch\> before doing anything else. Otherwise it remains on the current branch.
{{</ admonition >}}

It is practically vague, from only those two short notations. Therefore, a longer description available can be found [here](https://git-scm.com/docs/git-rebase).

How I understand it: Every single commit of the history of your current branch, is put on the top of the branch (latest commit) you are rebasing on. In other words, you have every commit in the stack:

&nbsp;



&nbsp;
