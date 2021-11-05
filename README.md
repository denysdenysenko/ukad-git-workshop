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

touch File1.txt
REM execute next command if you use cmd: type nul > File1.txt
git add File1.txt
git commit -m "Commit 1"

touch File2.txt
git add File2.txt
git commit -m "Commit 2"

touch WrongFile.txt
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
```
```
                                    _.-'~~~~~~~`-._
                                   /      | |      \
                                  /       | |       \
                                 | _______| |_______ |
                                 |/ ----- \_/ ----- \|
                                /  (     ) = (     )  \
                               / \  ----- ( ) -----  / \
                              /   \      /|||\      /   \
                             /     \    /|||||\    /     \
                            /       \  o=======o  /       \
                           /____,.--'     \ /     '--...___\
```
* Open GitHub, find your branch and  see that wrong commit has disappeared there too.

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
Amend functionality is very useful if you made accidental commit or you just want to add a tiny bit to the latest commit before pushing to the remote repository. Typical scenarios would be: update typo in commit message, add missing file etc. Amend doesn't work terribly well if you already pushed to the remote repository as it creates a new commit with new hash so would require force push.
Example of editing commit message:
```
git commit --amend -m "This is a correct commit message"
```

### Cherry-pick
Cherry-picking is a process of copying a commit from one branch to another. Please be aware that cherry-picking creates a separate commit with separate commit id, therefore for git it appears as a separate piece of work and might have tricky behaviour during branches merging. It works well if you want to copy some functionality from abandoned branch.
```
git checkout master
git checkout -b <your-prefix>/cherry-pick-test
git cherry-pick <commit hash>
```

### Multiline comments
Use multiline comments. Do not put sensitive information into the commit, but put enough data to be able to investigate commit origin in the future. A good example would be:
```
git commit -m "Task name goes to the first line
Particular details of what was done go to the second line
URL to the task tracker with more details goes to the third line"
```

## Merge vs Rebase vs Fast Forward
### Fast Forward 
Fast forward is a process of reassigning your branch label to some later commit if another commit is a direct descendant of current commit. In other words:
```
* Commit 3 - 1900-01-03 - some-other-branch /*you want this changes in your current branch*/
|
* Commit 2 - 1900-01-02
|
* Commit 1 - 1900-01-01 - your-current-branch /*you stay here*/
```
You can fast-forward without creating additional merge commit so that your-current-branch points to the Commit 3 in the same way as some-other-branch. Most of the time you'd want to use fast-forward whenever possible as it makes visual tree cleaner.

### Merge
Merge is a process of creating a new commit to unify changes from two different branches. Nothing special, run next steps to evaluate if needed:
```
git checkout master

git checkout -b <your-prefix>/merge-a
touch FileA.txt
git add *
git commit -m A


git checkout master

git checkout -b <your-prefix>/merge-b
touch FileB.txt
git add *
git commit -m B

git merge <your-prefix>/merge-a -m "Merge A into B"

git log --oneline --graph --all
```

### Rebase
Let's imagine this time that we have exactly the same scenario, but instead of doing merge we now want to move our `branch-b` as if it was started from `branch-a` and not `master`.
Let's repeat all the previous steps, but this time let's review our log before the final step and after rebase is done:
```
git checkout master

git checkout -b test/rebase-a
touch FileA.txt
git add *
git commit -m A


git checkout master

git checkout -b test/rebase-b
touch FileB1.txt
git add *
git commit -m B1
touch FileB2.txt
git add *
git commit -m B2

git log --oneline --graph --all
```

Now we can do the magic trick to move our commits through repository with the help of rebase command.
The syntax can be read as follows:
```
REM rebase --onto <the new place> <from the old place> <my branch named>
git rebase --onto <your prefix>/rebase-a master <your-prefix>/rebase-b
```
Or simpler versiod would be:
```
REM rebase my current branch onto the specified branch
git rebase <your prefix>/rebase-a
```
Query log to see the difference:
```
git log --oneline --graph --all
```
Rebasing is harder in some cases, but the great outcome is that you can have a clean straight repository history that's easy to navigate through.

## Multiple origins
You can work with git repositoryies using different topoligies, where you can have a single server as your team's main origin, multiple servers, chain servers etc.
### Two remote servers
* Create your own git repository, leave it empty and not initialized. In my case I created repository at https://github.com/denysdenysenko/mess-up-with-git-second-origin.git.
* Execute next command to add this new repository as your additional remote server:
```
git remote add second-origin https://github.com/denysdenysenko/mess-up-with-git-second-origin.git
git remote -v
```
* If you open your new remote server on GitHub you'd see it's empty. Let's push some work there:
```
git checkout master
git push second-origin
```
* If you open your new remote repository now you'd see that it contains exactly the same commit as the initial origin has. Content and ids or commits will also match.
* Let's now check how we can see if certain commit has got into a specific remote. Execute `git log` and review branch names list in parentheses:
 - _HEAD ->  master_ - my current local branch with current position on this commit;
 - _origin/master_ - same commit identified as branch tip in origin;
 - _second-origin/master_ - same commit identified as branch tip in second origin;
 - _origin/HEAD_ - commit identified as current in origin.

* Now let's push our branch _<your-prefix>/rebase-b_ to first origin, remove it locally, then pull it back and push to a second-origin. Execute steps one at a time to follow along:
```
git checkout <your-prefix>/rebase-b
git push origin
git checkout master
git branch --delete <your-previx>/rebase-b
git checkout --track origin/<your-prefix>/rebase-b
git push second-origin
```
* Revisit second repository on GitHub once again.

Using this approach you can pull from one repository, push to another repository, set code server in between, mimik forking etc.