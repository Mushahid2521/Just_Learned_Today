## Git and Github  
Git is a version control system or VCS which keeps track of code and configuration developed by Linus Torvold on 2005 when building Linux Kernel. It can be also called Source Controlled Management (SCM).     

  
### Diff  
Diff is used to find the difference between two files.   
```
diff old_file.py new_file.py
```  
  
To get a visible difference with + - symbol use 
```
diff -u old_file.py new_file.py 
```  

Other diff tools include KDiff, vimdiff etc.    
Some tools shows difference side by side highlighting the difference by color like   
```KDiff3, meld, vimdiff```  

### Patch File  
Storing the difference in a file is called patch file or diff file. We can do it by.  
```diff -u old_file new_file > change.diff```    
  
### Correcting a file or Patching  
To patch a old file we use diff file to make the change.  
```patch old_file < change.diff```  
  
## Git  
Setting Git config means setting name and email for the repo. Git also tracks who made the changes so it requires id.  
```
git config --global user.email 'me@example.com'  
git config --global user.name 'My name'  
```  

Git Commands
```
git init 
git status //shows if any file wasnt staged
git add .
git commit -m 'New change'   
git status
git log
```  

Git commit message structure 
```
Add a main line not more than 50 char

Gaping one line then multiple paragraph....
......

Another para ........
........  
```  
  
#### Skipping staging area
```commandline
git commit -a -m 'Commit Message'
```    

> HEAD alias indicate the current checked-out-snapshot  
  
  
#### History of changes in each commit  
```
git log -p
```  
It shows all the changes like ```diff -u``` style.   
To check change in particular commit. We need to first get the commit id and then show it.   
```
git log
git show commit_id
```     
  
#### Git Stat   
To see how many lines are added and removed we can use ```git log --stat```  to get the summary.  
   
#### Tracking change before commit  
Its useful when its required to see what new modification has been added from the previous commit. It shows the change in diff -u format.  
```
git diff
or
git diff filename
```
While staging we can also see the changes by using ```-p``` flag. This will ask if we want to stage the chnages.  
```
git add -p
```    

#### Removing and renaming file  
```
git rm filename
```  
To remove both from directory and from git. This needs to be done before staging.  

```
git mv oldname newname
```  

#### Ignoring file or files  
To do this we need to create a ```.gitignore``` file.   
```
echo .DS_STORE > .gitignore
```  
  
#### Revert a file to previous 
```
git checkout filename
```    
This revert the file to the previously commited file. With ```-p``` flag it ask for.  
  
#### Undo the Staged file  
To exclude a file from staging area we can reset the stage using reset command.  
```
git add *
git reset HEAD filename
```   
Now we are ready to commit.  

#### Amending Commit  
If a file missed in a commit that was supposed to be in that commit then we can amend the commit as follow. It overwrites the commit.   
```
git add left_file
git commit --amend
```    
This will allow to rewrite the commit message as well. But it rewrites the commit history which can make confusion to other collaborators.    
  
> Avoid amending commit that has been made public or pushed to remote.  
  
#### Rollback  
Git ```revert``` command makes a new commit by undoing the changes in the last commit. This helps us keep track what was the reason for the rollback and what was the changes made us to rollback.  
Head means to check the last commit.  
```
git revert HEAD

git log -p -2 //shows last two change
```         

> Git uses SHA1 algorithm to generate the commit id. SHA1 takes some information as input and generate 40 character long id. So when someone changes something intentionally then Git can spot the change that its not match the change with that commit id. This also ensures a unique id for each commit all over. So the possibility of generating a same commit id is very very low.  
  
  
We can rollback to specific commit by passing commit id.  
```
git log -2
git show id_4-8_char_enough
git revert id id_4-8_char_enough
```    
![Git](/Images/git_command.png)  
  
### Git Branch  
A pointer to a particular commit.    
```
git branch     // list all brances
git branch new-branch         // create new branch
git checkout new-branch       // Switch to new Branch
git checkout -b another-branch    // create and switch to that branch
```     
  
Deleting branch  
```
git branch -d branch_name
```    
If there is a chnage in this branch that is not merged then git will show an error.  
  
### Merging  
Combining branch data and history together. To merge a branch to the current branch:  
```
git merge branch-name
```    
  
Git uses two algorithm to perform merge.  
1. Fast-forward: Just update the pointer of the branch to the branch being merged.    
2. Three-way merge: When diffrent branch goes through different changes or commits and needs to be merged. Git combines two changes if occured in different places of same file. If not then it raise merge conflicts.  
  
If there is a conflict we need to open the file and make the changes. Then do git add and then git commit.  
To check the log in graph format.  
```
git log --graph --oneline 
```     

If there is a conflict and we want to abort the change or reset to the previous state, use --abort.  
```
git merge --abort
```      


![git merge](/Images/git_merge.png)    

  
### Github  
Github is the remote hosting service for Git.  
```
git clone url
cd repo-name

git commit -a -m "Commit message"
git push
```    

For push and pull we need to provide credentials all the time. To avoid it for 15 minutes, we need to use credential.helper. Or use SSH-key pair  
  
```
git config --global credential.helper cache
```    

Getting all the information from remote repo.  
```
git remote show origin

git branch -r // showing the remote branches info
```    

#### Git fetch vs pull  
Git fetch gets the changes in the remote repository and allows to see the changes before merging. Gut pull gets the commit and merge it to the local master branch.  
```
git fecth

git log origin/master  

git status 

git merge origin/master  
```   

```
git pull // fetches and merges the current remote branch to the local branch
git log -p 1 // to see the one patch change

git remote show origin // to see the branches fetched
git checkout branch_name 
```  

#### Git remote branch 
Create a new branch locally and push that branch to the master branch.  
```
git checkout -b branch_name

git push -u origin branch_name
```  

#### Rebasing changes   
When we make a different branch and both the branch gets committed with new changes then it is required to perform a three way merge. Which makes it hard to track changes and resolving conflict.  
To solve this we can use ```rebase```. Rebasing makes the commit changes linear and applies the fast forward merges. It also helps to identify which commit first introduced the bug.    


Ex: We made a new branch refactor. The master branch in the remote also changed by other developer. We now can not merge the master branch directly. So we do:  
```
git checkout master
git pull //new changes in the master can not be merged directly with refactor branch  

git checkout refactor
git rebase master

git log --graph --oneline 

git checkout master
git merge refactor 

// now we can delete the refactor branch
git push --delete origin refactor
git branch -d refactor

// now we are ready to push
git push
```  

Another Ex:  
We are working with local master branch made some changes and the remote master branch also changed with new commits.  

```
// star by doing a commit
git commit -a -m 'Commit message'

git fetch
git rebase origin/master      // solve the merge conflict here if any 

git add .
git rebase --continue 

git push
```  

### Best Practices for collaborating with others  

1. Synchronize the branch before starting.  
2. Avoid having lot of changes that modifies a lot of different things.  
3. Create a new feature branch working in a big changes.  
4. Regularly merge changes made on the master branch back on to the feature branch.  
5. Shouldn't rebase changes that has been pushed to remote repo.  


[Merge Conflict Solving](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/about-merge-conflicts)  
[Resolving a merge conflict using command line](https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/resolving-a-merge-conflict-using-the-command-line)  
  
    




  
  

  

  
  