# ukad-git-workshop
## Before you get started
Make sure you have a recent version of git client installed locally. You can download it from [here](https://git-scm.com/downloads).
You might also want to download necessary UI tools to visualize your repository structure throughout the workshop.
Please notice that many commands can be executed by providing full parameter name, like ```git push --force``` or by providing short versions of parameter names prefixed with single `-`, like ```git push -f```. We will use both versions throughout the workshop.

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

git log --oneline
```
* Revisit log output, be aware that remote repository contains a copy of your local branch.
* Reset HEAD of your current repository to one commit back
```
git reset --hard HEAD~1
cls
git log --oneline
```
Ensure that wong commit has desappeared from results.
* Try pushing to the remote repository, revisit error message.
```
git push
```
* Forcibly push your branch to the remote repository, this will reset branch pointer to the same commit like in your local copy.
```
git push --force
```
* Open your branch in GitHub to see that wrong commit has disappeared there too.

## Squash
Let's suppose that two commits that we made before should really be one. What you can use to achive the goal is so-called interactive rebase. Once interactive rebase is started, you will get a text file, containing commits to be updated.
Using interactive rebase you can reorder, combine and remove commits by simply editing the file. Once file is saved - rebase is completed and changes are applied to the repository.
If you failed along the way, just call ```git rebase --abort``` to start over again.
So, to combine two commits into one you have to SQUASH your commits, in order to do this initiate interactive rebase:
```
git rebase -i HEAD~2
```
In text editor that opens replace word "pick" with word "squash" on the second line. Save and close the file. Edit comments when second text editor prompts, save and close.
Force push once again ```git push -f``` and go to GitHub, notice that there's a single commit and content from both commits is present.

## Stash, amend, cherry-pick, multiline comments

## Merge vs Rebase vs Fast Forward

## Multiple origins

