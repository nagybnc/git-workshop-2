---
theme: seriph
class: "text-center"
background: ""
highlighter: shiki
colorSchema: "light"
lineNumbers: true
info: |
    ## Git workshop
    A hands-on workshop to learn the basics of Git
drawings:
    persist: false
css: unocss
---

# #2 Git Workshop

A hands-on workshop to learn the basics of Git

<div class="abs-br m-6 text-gray-600 flex items-center">
<p class="font-mono">NAGYBNC</p>
  <a href="https://github.com/nagybnc" target="_blank" alt="GitHub"
    class="text-xl icon-btn !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

---


# #2 workshop

-   Git Workflow
    -   Semantic Versioning
-   Cherry pick
-   Merge
-   Rebase
    -   Squash
-   Merge Conflict
-   Alias
-   Amend
-   Revert vs Reset
-   Hooks
-   Log
-   Exercises

---

# Gitflow Workflow


<img src="/images/git-workflow.svg" class="w-[600px] mx-auto"/>

<!--

1. A develop branch is created from main
2. A release branch is created from develop
3. Feature branches are created from develop
4. When a feature is complete it is merged into the develop branch
5. When the release branch is done it is merged into develop and main
6. If an issue in main is detected a hotfix branch is created from main
7. Once the hotfix is complete it is merged to both develop and main

-->

---

# Semantic Versioning 2.0.0

Given a version number MAJOR.MINOR.PATCH, increment the:

1. **MAJOR** version when you make incompatible API changes
2. **MINOR** version when you add functionality in a backwards compatible manner
3. **PATCH** version when you make backwards compatible bug fixes

Additional labels for pre-release and build metadata are available as extensions to the MAJOR.MINOR.PATCH format.

---

# Cherry pick

```shell {all}
git cherry-pick <sha>
```
<figure>
  <img src="/images/cherry-pick.png" class="mx-auto"/>
  <figcaption class="text-center">
    <a href="https://git-school.github.io/visualizing-git/#free-remote" target="_blank">https://git-school.github.io/visualizing-git/#free-remote</a>
  </figcaption>
</figure>

<!--

Cherry picking is the act of picking a commit from a branch and applying it to another. git cherry-pick
git cherry-pick is a useful tool but not always a best practice. Cherry picking can cause duplicate commits and many scenario.

For example, say a commit is accidently made to the wrong branch. You can switch to the correct branch and cherry-pick the commit to where it should belong.
developer has started work on a new feature. During that new feature development they identify a pre-existing bug. The developer creates an explicit commit patching this bug. This new patch commit can be cherry-picked directly to the main branch

-->

---

# Merge

<img src="/images/forked-commit-history.svg" class="w-[500px] mx-auto"/>

```shell {all}
git checkout feature
git merge main

-- OR --

git merge feature main
```


<!--

Rebase vs merge - Both of these commands are designed to integrate changes from one branch into another branch—they just do it in very different ways.

Consider what happens when you start working on a new feature in a dedicated branch, then another team member updates the main branch with new commits. 
The new commits in main are relevant to the feature that you’re working on

-->

---

# Merge

<img src="/images/merge-main-to-feature.svg" class="w-[600px] mx-auto"/>


<!--

This creates a new “merge commit” in the feature branch that ties together the histories of both branches
The feature branch will have an extraneous merge commit every time you need to incorporate upstream changes

-->


---

# Rebase

```shell {all}
git checkout feature
git rebase main
```

<img src="/images/rebase-feature-to-main.svg" class="w-[500px] mx-auto"/>


<!--

Rebasing re-writes the project history by creating brand new commits for each commit in the original branch.
This moves the entire feature branch to begin on the tip of the main branch, effectively incorporating all of the new commits in main
- Get a much cleaner project history.
- Eliminates the unnecessary merge commits.
- Perfectly linear project history

**Interactive Rebasing**
Typically, this is used to clean up a messy history before merging a feature branch into main.
like: Merge 2 commit into one.
adding the -i option to git rebase allows you to run rebase interactive

-->

---

# Rebase - when not to do it?

- The golden rule of git rebase is to never use it on public branches

<img src="/images/rebasing-the-main.svg" class="w-[500px] mx-auto"/>

<!--

Always ask yourself, “Is anyone else looking at this branch?

If you try to push the rebased main branch back to a remote repository, Git will prevent you from doing so because it conflicts with the remote main branch. But, you can force the push to go through by passing the --force flag

-->

---

# Squash
- To "squash" in Git means to combine multiple commits into one.

<img src="/images/reset-revert-checkout-squash.png" class="w-[600px] mx-auto"/>

<!--

There is no such thing as a stand-alone git squash command. Instead, squashing is rather an option when performing other Git commands like interactive rebase or merge.

-->

---

# Merge Conflict


### **Git fails to start the merge**

- Git sees there are changes in either the working directory or staging area of the current project
- Use git stash, git checkout, git commit or git reset

```shell {all}
error: Entry '<fileName>' not uptodate. Cannot merge. (Changes in working directory)
```

<br/>

### **Git fails during the merge**

- This indicates a conflict with another developers code

```shell {all}
error: Entry '<fileName>' would be overwritten by merge. Cannot merge. (Changes in staging area)
```

<!--

Sometimes multiple developers may try to edit the same content
Most of the time, Git will figure out how to automatically integrate new changes.

-->

---

# Alias

- Git aliases are a powerful workflow tool that create shortcuts to frequently used Git commands.
- Aliases are created through the use of the **git config** command

<br />

```shell {all}
$ git config --global alias.co checkout
$ git config --global alias.br branch
$ git config --global alias.ci commit
$ git config --global alias.st status
```

<br />

```shell {all}
    [alias]
        co = checkout
            br = branch
            ci = commit
            st = status
```
<!--

    Git aliases are a powerful workflow tool that create shortcuts to frequently used Git commands.
    Using Git aliases will make you a faster and more efficient developer

-->

---

# Changing the Last Commit

<div grid="~ cols-2 gap-4">

<div>

```shell {all}
git commit --amend -m "an updated commit message"
```

<br />

```shell {all}
# Edit hello.js and main.js
git add hello.js
git commit 
# Realize you forgot to add the changes from main.js
git add main.js
git commit --amend --no-edit
```

</div>

<img src="/images/changing-the-last-commit.svg" class="w-[400px] mx-auto"/>

</div>

<!--

- Amended commits are actually entirely new commits and the previous commit will no longer be on your current branch
- During a rebase, the edit or e command will pause the rebase playback on that commit and allow you to make additional changes with git commit --amend

-->

---

# Revert vs Reset

<div grid="~ cols-2 gap-4">

<div>

### **Revert**
- Used for undoing changes to a repository's commit history
- Also takes a specified commit, however, git revert does not move ref pointers to this commit.
- A safer alternative to git reset in regards to losing wor

<br />

### **Reset**
- A complex and versatile tool for undoing changes.
- Will move the HEAD ref pointer and the current branch ref pointer.

</div>


<img src="/images/reset-revert.svg" class="w-[400px] mx-auto"/>

</div>

<!--

Instead of deleting or orphaning commits in the commit history, a revert will create a new commit that inverses the changes specified. Git revert is a safer alternative to git reset in regards to losing work

-->

---

# Git Hooks

Git hooks are scripts that run automatically every time a particular event occurs in a Git repository.

- **pre-commit**: Check the commit message for spelling errors.
- **pre-receive**: Enforce project coding standards.
- **post-commit**: Email/SMS team members of a new commit.
- **post-receive**: Push the code to production.

<br />
<div v-click class="text-sm">
applypatch-msg |
pre-applypatch |
post-applypatch |
pre-commit |
prepare-commit-msg |
commit-msg |
post-commit |
pre-rebase |
post-checkout |
post-merge |
pre-receive |
update |
post-receive |
post-update |
pre-auto-gc |
post-rewrite |
pre-push
</div>
<br />

<div v-click>

```shell {all}
#!/bin/sh

echo "# Please include a useful commit message!" > $1
```

</div>
---

# Git hooks

<img src="/images/git-hooks.svg" class="w-[600px] mx-auto"/>


---

# Git log

Git log is a utility tool to review and read a history of everything that happens to a repository.

- **A commit hash**, which is a 40 character checksum data generated by SHA (Secure Hash Algorithm) algorithm. It is a unique number.
- **Commit Author metadata**: The information of authors such as author name and email.
- **Commit Date metadata**: It's a date timestamp for the time of the commit.
- **Commit title/message**: It is the overview of the commit given in the commit message.

<br />

<div grid="~ cols-2 gap-4">
<v-click>

```shell {all}
* commit d70a40d9f74ffdad3e1127b06ab80715fdd8d0bd (HEAD -> feature)
| Author: bence2.nagy <bence2.nagy@thyssenkrupp.com>
| Date:   Thu Oct 20 08:59:30 2022 +0200
|     teszt.js added
|
* commit fec8f23880eeabc2736bbcb7673b25841d900d37 (master)
  Author: bence2.nagy <bence2.nagy@thyssenkrupp.com>
  Date:   Mon Oct 10 11:14:57 2022 +0200
      Initial commit
```
</v-click>
<v-click>

```shell {1|3-6|7-10}
git log [<options>] [<revision-range>] [[--] <path>…​]

git log --oneline
git log --oneline --decorate
git log --graph --oneline --decorate

git log --after="yy-mm-dd"
git log --after="2019-11-01" --before="2019-11-08 "
git log --author="Author name"
git log --grep=" Commit message."
```
</v-click>
</div>

---

# #1 Exercise

### **Change a letter case in the filename of an already tracked file**

You have committed a **File.txt** but then you realized the filename should be all lowercase: **file.txt**. Change the filename.

This one is tricky on Windows, or in any filesystem that treats **File.txt** and **file.txt** as the same files.

<v-click>

**Hint:**
```shell {all}
Try moving the file with git mv instead of just move
```
</v-click>

<v-click>

**Solution:**
```shell {all}
git mv File.txt file.txt
git commit -am "Lowercase file.txt"
```
</v-click>

---

# #2 Exercise

### **Fix typographic mistake in the last commit**

You have committed **file.txt** but you realized you made a typo - you wrote **wordl** instead of **world**.

Edit previous commit so no one would realize you haven't checked the file before committing it.

Pay attention to the commit message, too!

<v-click>

**Hint:**
```shell {all}
You should use the git commit --amend
```

</v-click>

<v-click>

**Solution:**
```shell {all}
git commit -a --amend
```

</v-click>

<!--

-->

---

# #3 Exercise

### **Forge the commit's date**

You should have finished your work a week ago. However, you had some more important things to do so you have committed the work just now.

As a git expert, change the date of the last commit. Don't be modest - make it look like it was committed in 2000!

<v-click>

**Hint:**
```shell {all}
Maybe there are some date manipulation allowed during the git commit --amend?
```

</v-click>
<v-click>

**Solution:**
```shell {all}
git commit --amend --no-edit --date="2000-10-21"
```

</v-click>

---

# #4 Exercise

### **Fix typographic mistake in old commit**

While you were working you noticed a typographic error in **file.txt** - you wrote **wordl** instead of **world**. Unfortunately, you have made another commit on top of the typo so simple **git commit --amend** is not enough.

Fix the typographic error by amending commit in history.

<v-click>

**Hint:**
```shell {all}
You should use the git rebase command in the interactive mode
```

</v-click>
<v-click>

**Solution:**
```shell {all}
git rebase -i HEAD^^
# mark the first commit with \"edit\" command
# fix the typo in the file
git add file.txt
git rebase --continue
# fix the typo in the commit message\n"
```

</v-click>

---

# #5 Exercise

### **Too many commits**

You were working on an issue and created two commits introducing very small change. You don't want to mess up your project history so you want to make only one commit that contains changes made in the last two.

Execute **git log -2** to see last two commits.

<v-click>

**Hint:**
```shell {all}
You should use the git rebase --interactive to squash the commits.
```
</v-click>

<v-click>

**Solution:**
```shell {all}
git rebase -i HEAD^^
```
</v-click>

---

# #6 Exercise

### **Change order of commits**

You have committed two changes but you don't like the order in which they appear in the history. Switch them.

Execute **git log -2** to see last two commits.

<v-click>

**Hint:**
```shell {all}
You should use the git rebase --interactive (again!)
```
</v-click>

<v-click>

**Solution:**
```shell {all}
git rebase -i HEAD~2 or git rebase -i --root
```
</v-click>

---

# #7 Exercise

### **Find commits that introduced swearwords**

You noticed that your project contains swearwords. You don't want to remove them in the next commit as your boss might someday find out that they were present in the codebase for a while.

Find all commits that add shit word to either words.txt or list.txt and change them so they introduce word flower instead.

<v-click>

**Hint:**
```shell {all}
You should use one of the options of the git log command to find all commits that introduced a swearword.
Then, use interactive rebase to remove them.
```
</v-click>

<v-click>

**Solution:**
```shell {all}
git log -Sshit
git rebase -i <found commit>^
# remove the swearwords
git rebase --continue
```
</v-click>

---

# Questions?
