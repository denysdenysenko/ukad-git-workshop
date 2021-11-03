# ukad-git-workshop
## Before you get started
Make sure you have a recent version of git client installed locally. You can download it from [here](https://git-scm.com/downloads).
You might also want to download necessary UI tools to visualize your repository structure throughout the workshop.

## Force push
* Ask the host to grant you necessary repository permissions.
* Open console and run following commands (you might want to run them one by one to make sure you follow the process), replace placeholders whenever necessary:
```
git clone https://github.com/denysdenysenko/mess-up-with-git.git
cd mess-up-with-git

REM create a new branch
git branch test/force-push-test

REM switch to it
git checkout test/force-push-test

REM alternatively you can replace previous two commands with a single command
REM git checkout -b <your-fill-name-without-spaces>/force-push-test

type nul > File1.txt
git add File1.txt
git commit -m "Commit 1"

type nul > File2.txt
git add File2.txt
git commit -m "Commit 2"

type nul > WrongFile.txt
git add WrongFile.txt
git commit -m "Wrong file"

git push --set-upstream origin test/force-push-test

git log
```
* Revisit log output, be aware that remote repository contains a copy of your local branch.
* Reset HEAD of your current repository to one commit back
```
git reset --hard HEAD~1
cls
got log
```
Ensure that wong commit has desappeared from results.
* Try pushing to the remote repository, revisit error message.
```git push```
* Forcibly push your branch to the remote repository, this will reset branch pointer to the same commit like in your local copy.
```git push --force```
* Open your branch in GitHub to see that wrong commit has disappeared there too.

## Squash

## Stash

## Multiline comments

## Merge vs Rebase vs Fast Forward

## Multiple origins

