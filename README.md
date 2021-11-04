# ukad-git-workshop
## Before you get started
Make sure you have a recent version of git client installed locally. You can download it from [here](https://git-scm.com/downloads).
You might also want to download necessary UI tools to visualize your repository structure throughout the workshop.
Ask the host to grant you necessary repository permissions.
Please beware that many commands can be executed by providing full parameter names, like ```git push --force``` or by providing short versions of parameter names prefixed with single `-`, like ```git push -f```. We will use both versions throughout the workshop.

## Force push
* Open console and run following commands (you might want to run them one by one to make sure you follow the process), replace placeholders whenever necessary:
```
git clone https://github.com/denysdenysenko/mess-up-with-git.git
cd mess-up-with-git

REM create a new branch
git branch <your-prefix>/force-push-test

REM switch to it
git checkout <your-prefix>/force-push-test

REM alternatively you can replace previous two commands with a single command
REM git checkout -b <your-prefix>/force-push-test

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
* Revisit log output, be aware that remote repository now contains a copy of your local branch.
* Let's imagine that latest commit shouldn't have gone to the repository, let's try to remove it. First you have to get rid of it locally, to achieve that reset HEAD of your current repository to one commit back.
```
git reset --hard HEAD~1
cls
git log --oneline
```
Ensure that wong commit has desappeared from results.
* Try pushing to the remote repository, revisit the error message.
```
git push
```
* Forcibly push your branch to the remote repository, this will reset branch pointer to the same commit like in your local copy.
```
git push --force

REM              _.-'~~~~~~`-._
REM             /      ||      \
REM            /       ||       \
REM           |        ||        |
REM           | _______||_______ |
REM           |/ ----- \/ ----- \|
REM          /  (     )  (     )  \
REM         / \  ----- () -----  / \
REM        /   \      /||\      /   \
REM       /     \    /||||\    /     \
REM      /       \  /||||||\  /       \
REM     /_        \o========o/        _\
REM       `--...__|`-._  _.-'|__...--'
REM               |    `'    |
```
* Open your branch in GitHub to see that wrong commit has disappeared there too.

## Squash
Let's suppose that two commits that we made before should really be one. What you can use to achive the goal is so-called interactive rebase. Once interactive rebase is started, you will get a text file, containing commits to be updated.
Using interactive rebase you can reorder, combine and remove commits by simply editing the file. Once the file is saved - rebase is completed and changes are applied to the repository.
If you failed along the way, just call ```git rebase --abort``` to start over again.
So, to combine two commits into one you have to *squash* them, in order to do this initiate interactive rebase like this:
```
git rebase -i HEAD~2
```
In text editor that opens replace word "pick" with word "squash" on the second line. Save and close the file. Edit comments when second text editor prompts, save and close.
Force push once again ```git push -f``` and go to GitHub, notice that there's a single commit and content from both commits is present.

## Stash, amend, cherry-pick, multiline comments
### Stash
Stash is a process of putting your work in progress aside for some time, you can assign names to stashes or keep them very basic. Most common scenario to use stashes is:
- Your repo is a mess, you don't want to commit anything at that point.
- Your colleague asks you to switch to other code branch (to merge, commit etc).
- You stash your current state of the repository, do whatever you have to do, stash everything back and continue what you did initially.
Another scenario could be - you accidentally start making your changes while staying on a wrong branch, right before you are ready to commit you figure out that you need another branch, you stash everything, switch to a correct branch, apply stash, commit to a correct branch, in some scenarios this might be an easier alternative to rebasing. 
```
git stash
git checkout correct-branch-name
git stash apply
git commit -m "This is a correct branch to commit now"
```
### Amend
Amend functionality is very useful if you made accidental commit or you just want to add a tiny bit to the latest commit before pushing to the remote repository. Typical scenarios would be: update typo in commit message, add missing file etc. Amend doesn't work terribly well if you already pushed to the remote repository as it creates a new commit with new hash so would requuire force push.
And example of editing commit message:
```
git commit --amend -m "This is a correct commit message"
```

### Cherry-pick
Cherry-picking is a process of copying a commit from one branch to another. Please be aware that cherry-picking creates a separate commit with separate commit id, therefore for git it appears as a separate piece of work and might have tricky behaviour during branches merging. It's good if you want to copy some functionality from abandoned branch.
```
git checkout master
git checkout -b <your-prefix>/cherry-pick-test
git cherry-pick <commit hash>
```

### Multiline comments
Use multiline comments. Do not put sensitive information into the commit, but put enough data to be able to investigate commit origin in the future. A good example would be:
```
git commit -m "Task name goes to the first line
Particular details of what was done
URL to the task tracker with more details"
```

## Merge vs Rebase vs Fast Forward

## Multiple origins

