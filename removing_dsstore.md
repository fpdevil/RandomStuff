# Getting rid of the .DS_Store from git commits.

On `macOS` operating system, the `.DS_Store` (_Desktop Services Store_) is a file which stores some custom attributes of its enclosing folder, such as the position of icons, any special files or images.

Its automatically created and maintained by the `Finder` application for each folder created under `macOS`. These files are simply a menace when it comes to version control, as they change too frequently and appear as uncommitted changes forcing us to commit every time, unless they are excluded with an entry in the `.gitignore` file upfront while creating the repository.

If we miss black listing them during the initial commit step, these may pile up in the repository as a part of the continuous integration process, until we realize the junk. Nevertheless they can be cleared later at any step as below.

1. First find and remove the existing `.DS_Store` files using the `find` and `git rm` combination as below:

```bash
⇒  find . -name ".DS_Store" -exec git rm --ignore-unmatch {} \;
rm 'project-x/.DS_Store'
rm 'project-x/systems/.DS_Store'
rm 'project-x/command/controller/navigation/.DS_Store'
```

2. Now check status and commit the changes.

```bash
⇒  git status
On branch master
Your branch is up to date with 'origin/master'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
    deleted:    project-x/.DS_Store
    deleted:    project-x/systems/.DS_Store
    deleted:    project-x/command/controller/navigation/.DS_Store

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	.gitignore
```

3. Add an entry into the `.gitignore` to ignore `.DS_Store` as below for project specific settings

```bash
# adding the .DS_Store to ignore list
echo .DS_Store >> .gitignore
```

4. Commit the changes

```bash
⇒  git add -A .gitignore

⇒  git commit -m "removed the .DS_Store dirs."
[master 3e8fge385] removed the .DS_Store dirs.
 3 files changed, 0 insertions(+), 0 deletions(-)
 delete mode 100644 project-x/.DS_Store
 delete mode 100644 project-x/systems/.DS_Store
 delete mode 100644 project-x/command/controller/navigation/.DS_Store

⇒  git push -u origin master
Enumerating objects: 11, done.
Counting objects: 100% (11/11), done.
Delta compression using up to 8 threads
```

## Blacklisting `.DS_Store` globally.

In case if we would like to ignore the `.DS_Store` file inclusion by default globally for all projects, then we can simply create a global rule for the same as below.

```bash
# specifying a global git exclusion list
⇒ echo ".DS_Store" >> ~/.gitignore_global
⇒ echo "._.DS_Store" >> ~/.gitignore_global
⇒ echo "**/.DS_Store" >> ~/.gitignore_global
⇒ echo "**/._.DS_Store" >> ~/.gitignore_global
⇒ git config --global core.excludesfile ~/.gitignore_global
```
