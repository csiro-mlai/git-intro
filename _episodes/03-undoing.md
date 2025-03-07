---
layout: episode
title: Undoing things
teaching: 10
exercises: 15
questions:
  - How can I undo things?
objectives:
  - Learn to undo changes safely
  - See when changes are permanently deleted and when they can be retrieved
keypoints:
  - "Git history can be reverted without modifying it"
  - "Once changes are committed they are safe"
  - "Changes that are not committed can be deleted"
---

## Undoing things

The whole point of version control is that you can be see and retrieve previous snapshots of your work.

With Git, if you have committed (made a snapshot) of a file, you can get it back, even if it gets deleted or modified in the future.

Some commands will modify the hitory of a Git repository. This is generally a **very bad** idea, and you should only do it if you're really confident you know what you're doing.

If changes are **uncommitted**, they are not safe, and if they are deleted, they are gone.

---

### Reverting commits

- Imagine we made a few commits.
- We realize that the latest commit was a mistake and we wish to undo it:

```
$ git log --oneline

f960dd3 (HEAD -> master) not sure this is a good idea
40fbb90 draft a readme
dd4472c we should not forget to enjoy
2bb9bb4 add half an onion
2d79e7e adding ingredients and instructions
```

A safe way to undo the commit is to revert the commit with `git revert`:

```
$ git revert f960dd3
```

This creates a **new commit** that does the opposite of the reverted commit.
The old commit remains in the history:

```
$ git log --oneline

d62ad3e (HEAD -> master) Revert "not sure this is a good idea"
f960dd3 not sure this is a good idea
40fbb90 draft a readme
dd4472c we should not forget to enjoy
2bb9bb4 add half an onion
2d79e7e adding ingredients and instructions
```

> ## Challenge 1
>
> Make a new commit in your guacamole repo (it can be whatever you like)
> 
> Inspect the history, and revert the commit. 
{: .challenge}

---

### Undo unstaged/uncommitted changes

DANGER!! 

This is a command that **permanently deletes** changes
that haven't been staged/committed!

### Modify before staging

- Make a silly change to repo, do not stage it or commit it.
- Inspect the change with `git status` and `git diff`.
- Now undo the change with `git checkout <file>`.
- Verify that the change is gone with `git status` and `git diff`.

### Modify after staging

- Make a reasonable change to a project, stage it.
- Make a silly change after you have staged the reasonable change.
- Inspect the situation with `git status`, `git diff`, `git diff --staged`, and `git diff HEAD`.
- Now undo the silly change with `git checkout <file>`.
- Inspect the new situation with `git status`, `git diff`, `git diff --staged`, and `git diff HEAD`.

---

> ## Challenge 2
> 
> How much do you trust Git...?
> 
> Delete one (or more) of your committed files using any method you like.
> 
> Can you get it/them back?
> 
> > ## Solution to Challenge 2
> > 
> > As long as a file was committed, you can get it back with `git checkout <file>`, but Git isn't magic - you won't have any changes that weren't committed.
> {: .solution}
{: .challenge}

## Recovering a previous versions

Because git stores the complete history of whatever snapshots you have recorded, you can step back to anyone of them at different levels of detail, from the complete working directory, to single files, even to single changes within files.

To get back to a previous state for the whole working directory, you can use `git checkout [commit]`. This can be useful for having a look at files, but you'll get a warning about a detached HEAD (more discussion later).

### Recovering previous versions of single files

Often it's useful to be able to access a previous version of a particular file. When the `checkout` command is given a file path (along with a reference to a commit) it will update that path to the previous state.

Let's see that with our recipe. If we wanted to get `ingredients.txt` back to its state before the addition of the onion, we could run

```
git checkout 2d79e7e ingredients.txt

Updated 1 path from 2d79e7e
```

If you run `git status` you'll see that the changes to the file `ingredients.txt`, bringing it back to the previous state, are already staged and ready to be committed.

