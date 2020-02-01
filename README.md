# Git Guide

### Files Lifecycle

(1) Untracked (Does not exist as per Git) 

(2) Tracked Unmodified (No change from last Commit) 

(3) Tracked Modified (Change from last Commit)

(4) Staged (Add in Snapshot for Commit of Snapshot, though it is not committed)

(5) Commited (Commit point)


```

(1)                (2)                (3)                (4)
 |                                                        |
 ----------------------- git add ------------------------->
                                       |                  |
                                       ----- git add ----->
                                       |                  |
                                       <------ modify -----
		     |                 |
		     ----- modify ---->
                     |                                    |
                     <----------- git commit --------------

```


### 1. Init Repo

```
apples-MacBook-Air:TEST apple$ mkdir mygitguide
apples-MacBook-Air:TEST apple$ cd mygitguide/

apples-MacBook-Air:mygitguide apple$ ls -ltra
total 0
drwxr-xr-x   2 apple  staff   64 Feb  1 12:47 .
drwxr-xr-x  30 apple  staff  960 Feb  1 12:47 ..
apples-MacBook-Air:mygitguide apple$ git init
Initialized empty Git repository in /Users/apple/TEST/mygitguide/.git/
apples-MacBook-Air:mygitguide apple$ ls -ltra
total 0
drwxr-xr-x  30 apple  staff  960 Feb  1 12:47 ..
drwxr-xr-x   3 apple  staff   96 Feb  1 12:47 .
drwxr-xr-x  10 apple  staff  320 Feb  1 12:47 .git
apples-MacBook-Air:mygitguide apple$ 
```

### 2. Track Files

```
apples-MacBook-Air:mygitguide apple$ echo "Line1" > sample.txt
apples-MacBook-Air:mygitguide apple$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	sample.txt

nothing added to commit but untracked files present (use "git add" to track)
apples-MacBook-Air:mygitguide apple$ 

apples-MacBook-Air:mygitguide apple$ git add sample.txt 

apples-MacBook-Air:mygitguide apple$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   sample.txt

apples-MacBook-Air:mygitguide apple$
```


### 3. Commit

- Normal Commit

```
apples-MacBook-Air:mygitguide apple$ git commit -m "commit 1"
[master (root-commit) 8717e04] commit 1
 1 file changed, 2 insertions(+)
 create mode 100644 sample.txt
apples-MacBook-Air:mygitguide apple$
```

- Correct last commit

```
apples-MacBook-Air:mygitguide apple$ git log --pretty=oneline
e9f994b9bd16cf726a7b63af02231d6cf0bdc995 (HEAD -> master) commit 2
8717e04fa24fefa14241c2d822593852046eaa0a commit 1
apples-MacBook-Air:mygitguide apple$ echo 'Line6' >> sample.txt 
apples-MacBook-Air:mygitguide apple$ git add sample.txt 
apples-MacBook-Air:mygitguide apple$ 
apples-MacBook-Air:mygitguide apple$ git commit --amend -m 'Ammended 2'
[master 6e3e1a2] Ammended 2
 Date: Sat Feb 1 13:50:37 2020 +0100
 1 file changed, 3 insertions(+)
apples-MacBook-Air:mygitguide apple$ git log --pretty=oneline
6e3e1a2d2fd9fe775ffa92e4559a0d80d0c1a47d (HEAD -> master) Ammended 2
8717e04fa24fefa14241c2d822593852046eaa0a commit 1
apples-MacBook-Air:mygitguide apple$
```

- Rollback last commit (with Conflict)

```
apples-MacBook-Air:mygitguide apple$ git revert 6e3e1a2d2fd9fe775ffa92e4559a0d80d0c1a47d
error: could not revert 6e3e1a2... Ammended 2
hint: after resolving the conflicts, mark the corrected paths
hint: with 'git add <paths>' or 'git rm <paths>'
hint: and commit the result with 'git commit'

apples-MacBook-Air:mygitguide apple$ cat sample.txt 
Line1
Line2
<<<<<<< HEAD
Line3
Line5
Line6
Line7
=======
>>>>>>> parent of 6e3e1a2... Ammended 2
apples-MacBook-Air:mygitguide apple$ vim sample.txt 
apples-MacBook-Air:mygitguide apple$ cat sample.txt 
Line3
Line5
Line6
Line7

apples-MacBook-Air:mygitguide apple$ git add sample.txt 
apples-MacBook-Air:mygitguide apple$ git commit -m 'Ammended 4'
[master d6a8982] Ammended 4
 1 file changed, 2 deletions(-)
apples-MacBook-Air:mygitguide apple$ git log --pretty=oneline
d6a89829b0faeeddbe8c1147976466180a955dee (HEAD -> master) Ammended 4
56143596dc1ec3418b92a858173b5748e7985010 Ammended 3
6e3e1a2d2fd9fe775ffa92e4559a0d80d0c1a47d Ammended 2
8717e04fa24fefa14241c2d822593852046eaa0a commit 1
```

### 4. View changes

```
apples-MacBook-Air:mygitguide apple$ echo "Line3" >> sample.txt
apples-MacBook-Air:mygitguide apple$ git diff
diff --git a/sample.txt b/sample.txt
index ac2dd81..e921797 100644
--- a/sample.txt
+++ b/sample.txt
@@ -1,2 +1,3 @@
 Line1
 Line2
+Line3
apples-MacBook-Air:mygitguide apple$ git diff --staged
apples-MacBook-Air:mygitguide apple$ 
apples-MacBook-Air:mygitguide apple$ git add sample.txt 
apples-MacBook-Air:mygitguide apple$ 
apples-MacBook-Air:mygitguide apple$ git diff
apples-MacBook-Air:mygitguide apple$ 
apples-MacBook-Air:mygitguide apple$ git diff --staged
diff --git a/sample.txt b/sample.txt
index ac2dd81..e921797 100644
--- a/sample.txt
+++ b/sample.txt
@@ -1,2 +1,3 @@
 Line1
 Line2
+Line3
apples-MacBook-Air:mygitguide apple$ 
```

### 5. Ignore Tracking (.gitignore)

```
*.log -> Any file with name ending with log.
/*.log -> Any file with name ending with log, only in directory as the gitignore file.
/**/*.log -> Any file with name ending with log, in any directory.
*LOG*/ -> Any directory with name containing  name with string LOG.
```


### 6. Show History

```
apples-MacBook-Air:mygitguide apple$ git log
commit 743bc3970971cae73c48fe22f3cf09faf09fb797 (HEAD -> master)
Author: apple <sanmuk21@gmail.com>
Date:   Sat Feb 1 13:50:37 2020 +0100

    commit 2

commit 8717e04fa24fefa14241c2d822593852046eaa0a
Author: apple <sanmuk21@gmail.com>
Date:   Sat Feb 1 13:24:07 2020 +0100

    commit 1
apples-MacBook-Air:mygitguide apple$ 

apples-MacBook-Air:mygitguide apple$ git log -1
commit 743bc3970971cae73c48fe22f3cf09faf09fb797 (HEAD -> master)
Author: apple <sanmuk21@gmail.com>
Date:   Sat Feb 1 13:50:37 2020 +0100

    commit 2
apples-MacBook-Air:mygitguide apple$ 

apples-MacBook-Air:mygitguide apple$ git log -1 -p
commit 743bc3970971cae73c48fe22f3cf09faf09fb797 (HEAD -> master)
Author: apple <sanmuk21@gmail.com>
Date:   Sat Feb 1 13:50:37 2020 +0100

    commit 2

diff --git a/sample.txt b/sample.txt
index ac2dd81..e921797 100644
--- a/sample.txt
+++ b/sample.txt
@@ -1,2 +1,3 @@
 Line1
 Line2
+Line3
apples-MacBook-Air:mygitguide apple$

apples-MacBook-Air:mygitguide apple$ git log -1 --pretty=oneline
743bc3970971cae73c48fe22f3cf09faf09fb797 (HEAD -> master) commit 2

apples-MacBook-Air:mygitguide apple$ git log -1 --since '15 minutes' --pretty=oneline
743bc3970971cae73c48fe22f3cf09faf09fb797 (HEAD -> master) commit 2

apples-MacBook-Air:mygitguide apple$ git log -1 --until '15 minutes' --pretty=oneline
8717e04fa24fefa14241c2d822593852046eaa0a commit 1

apples-MacBook-Air:mygitguide apple$ git log --graph --pretty=oneline
* 743bc3970971cae73c48fe22f3cf09faf09fb797 (HEAD -> master) commit 2
* 8717e04fa24fefa14241c2d822593852046eaa0a commit 1
```

### 7. Clone Remote

```
apples-MacBook-Air:TEST apple$ git clone https://github.com/sankamuk/mygitguide.git
Cloning into 'mygitguide'...
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), done.
apples-MacBook-Air:TEST apple$

apples-MacBook-Air:TEST apple$ cd mygitguide
apples-MacBook-Air:mygitguide apple$ git log --pretty=oneline
dd1f9750793c026b11f98f471fcd1802aeab7a57 (HEAD -> master, origin/master, origin/HEAD) Initial commit
apples-MacBook-Air:mygitguide apple$

apples-MacBook-Air:mygitguide apple$ cp ../mygitguide-local/sample.txt .
apples-MacBook-Air:mygitguide apple$ git add sample.txt 
apples-MacBook-Air:mygitguide apple$ git commit -m 'commit 2'
[master adf7d9e] commit 2
 1 file changed, 4 insertions(+)
 create mode 100644 sample.txt
apples-MacBook-Air:mygitguide apple$ git log --pretty=oneline
adf7d9ef24e6fd157cde66deb07d17b2de1ec66f (HEAD -> master) commit 2
dd1f9750793c026b11f98f471fcd1802aeab7a57 (origin/master, origin/HEAD) Initial commit

apples-MacBook-Air:mygitguide apple$ git remote 
origin
apples-MacBook-Air:mygitguide apple$ git fetch origin
apples-MacBook-Air:mygitguide apple$ git merge origin/master master
Already up to date.
apples-MacBook-Air:mygitguide apple$ git log --pretty=oneline
adf7d9ef24e6fd157cde66deb07d17b2de1ec66f (HEAD -> master) commit 2
dd1f9750793c026b11f98f471fcd1802aeab7a57 (origin/master, origin/HEAD) Initial commit

apples-MacBook-Air:mygitguide apple$ git push origin master
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 4 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 285 bytes | 285.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/sankamuk/mygitguide.git
   dd1f975..adf7d9e  master -> master


apples-MacBook-Air:mygitguide apple$ git remote show origin
* remote origin
  Fetch URL: https://github.com/sankamuk/mygitguide.git
  Push  URL: https://github.com/sankamuk/mygitguide.git
  HEAD branch: master
  Remote branch:
    master tracked
  Local branch configured for 'git pull':
    master merges with remote master
  Local ref configured for 'git push':
    master pushes to master (up to date)
```


### 8. Tag

```
apples-MacBook-Air:mygitguide apple$ git tag
apples-MacBook-Air:mygitguide apple$ git tag -a 'v1' -m 'Tag v1'
apples-MacBook-Air:mygitguide apple$ git tag
v1
apples-MacBook-Air:mygitguide apple$ git show 'v1'
tag v1
Tagger: apple <sanmuk21@gmail.com>
Date:   Sat Feb 1 14:44:45 2020 +0100

Tag v1

commit adf7d9ef24e6fd157cde66deb07d17b2de1ec66f (HEAD -> master, tag: v1, origin/master, origin/HEAD)
Author: apple <sanmuk21@gmail.com>
Date:   Sat Feb 1 14:32:36 2020 +0100

    commit 2

diff --git a/sample.txt b/sample.txt
new file mode 100644
index 0000000..33cdce4
--- /dev/null
+++ b/sample.txt
@@ -0,0 +1,4 @@
+Line3
+Line5
+Line6
+Line7


apples-MacBook-Air:mygitguide apple$ git push origin 'v1'
Enumerating objects: 1, done.
Counting objects: 100% (1/1), done.
Writing objects: 100% (1/1), 153 bytes | 153.00 KiB/s, done.
Total 1 (delta 0), reused 0 (delta 0)
To https://github.com/sankamuk/mygitguide.git
 * [new tag]         v1 -> v1
```

### 9. Branching

- State 1

```
             master (HEAD)
                |     |
dd1f975079- -> adf7d9ef24-


apples-MacBook-Air:mygitguide apple$ git log --pretty=oneline
adf7d9ef24e6fd157cde66deb07d17b2de1ec66f (HEAD -> master, tag: v1, origin/master, origin/HEAD) commit 2
dd1f9750793c026b11f98f471fcd1802aeab7a57 Initial commit
```

- State 2

```
             master (HEAD)
                |     |
dd1f975079- -> adf7d9ef24-
                   |
               new-branch


apples-MacBook-Air:mygitguide apple$ git branch new-branch
apples-MacBook-Air:mygitguide apple$ git log --pretty=oneline
adf7d9ef24e6fd157cde66deb07d17b2de1ec66f (HEAD -> master, tag: v1, origin/master, origin/HEAD, new-branch) commit 2
dd1f9750793c026b11f98f471fcd1802aeab7a57 Initial commit
apples-MacBook-Air:mygitguide apple$ git checkout new-branch
Switched to branch 'new-branch'
apples-MacBook-Air:mygitguide apple$ git log --pretty=oneline
adf7d9ef24e6fd157cde66deb07d17b2de1ec66f (HEAD -> new-branch, tag: v1, origin/master, origin/HEAD, master) commit 2
dd1f9750793c026b11f98f471fcd1802aeab7a57 Initial commit
```

- State 3

```
             master
                |     
dd1f975079- -> adf7d9ef24- -> 77d31c9720-
                               |      |
                         new-branch  (HEAD)


apples-MacBook-Air:mygitguide apple$ echo "Line8" >> sample.txt 
apples-MacBook-Air:mygitguide apple$ git add sample.txt 
apples-MacBook-Air:mygitguide apple$ git commit -m 'commit 3'
[new-branch 77d31c9] commit 3
 1 file changed, 1 insertion(+)
apples-MacBook-Air:mygitguide apple$ git log --pretty=oneline
77d31c972051ff807bd43fc09919ca46abee6fb1 (HEAD -> new-branch) commit 3
adf7d9ef24e6fd157cde66deb07d17b2de1ec66f (tag: v1, origin/master, origin/HEAD, master) commit 2
dd1f9750793c026b11f98f471fcd1802aeab7a57 Initial commit
```

- State 3

```
                            master  (HEAD)
                                |     |
                            -> 32c650f1a8-
dd1f975079- -> adf7d9ef24- |
                            -> 77d31c9720-
                               |      
                             new-branch  


apples-MacBook-Air:mygitguide apple$ git checkout master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.
apples-MacBook-Air:mygitguide apple$ git log --pretty=oneline
adf7d9ef24e6fd157cde66deb07d17b2de1ec66f (HEAD -> master, tag: v1, origin/master, origin/HEAD) commit 2
dd1f9750793c026b11f98f471fcd1802aeab7a57 Initial commit
apples-MacBook-Air:mygitguide apple$ echo "Line9" >> sample.txt 
apples-MacBook-Air:mygitguide apple$ git add sample.txt 
apples-MacBook-Air:mygitguide apple$ git commit -m 'commit 4'
[master 32c650f] commit 4
 1 file changed, 1 insertion(+)
apples-MacBook-Air:mygitguide apple$ git log --pretty=oneline
32c650f1a80b4907e44ec60c16ed09933cc7dcdf (HEAD -> master) commit 4
adf7d9ef24e6fd157cde66deb07d17b2de1ec66f (tag: v1, origin/master, origin/HEAD) commit 2
dd1f9750793c026b11f98f471fcd1802aeab7a57 Initial commit
```

- State 4
        
```        
                                            master  (HEAD)
                            -> 32c650f1a8-     |      |
dd1f975079- -> adf7d9ef24- |              |-> 32c650f1a8-
                            -> 77d31c9720-
                               |      
                         new-branch  


apples-MacBook-Air:mygitguide apple$ git branch
* master
  new-branch
apples-MacBook-Air:mygitguide apple$ git merge new-branch
Auto-merging sample.txt
CONFLICT (content): Merge conflict in sample.txt
Automatic merge failed; fix conflicts and then commit the result.
apples-MacBook-Air:mygitguide apple$ git status
On branch master
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)

You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)

	both modified:   sample.txt

no changes added to commit (use "git add" and/or "git commit -a")
apples-MacBook-Air:mygitguide apple$ git checkout new-branch
sample.txt: needs merge
error: you need to resolve your current index first
apples-MacBook-Air:mygitguide apple$ git branch
* master
  new-branch

apples-MacBook-Air:mygitguide apple$ vim sample.txt 
apples-MacBook-Air:mygitguide apple$ git add sample.txt 
apples-MacBook-Air:mygitguide apple$ git commit -m 'commit 5'
[master 8bcde89] commit 5

apples-MacBook-Air:mygitguide apple$ git log --pretty=oneline
8bcde8905a842fcc0992bfb48191d57610318c51 (HEAD -> master) commit 5
32c650f1a80b4907e44ec60c16ed09933cc7dcdf commit 4
77d31c972051ff807bd43fc09919ca46abee6fb1 (new-branch) commit 3
adf7d9ef24e6fd157cde66deb07d17b2de1ec66f (tag: v1, origin/master, origin/HEAD) commit 2
dd1f9750793c026b11f98f471fcd1802aeab7a57 Initial commit

apples-MacBook-Air:mygitguide apple$ git branch -d new-branch
Deleted branch new-branch (was 77d31c9).
```

### 10. Remote branching

```
apples-MacBook-Air:mygitguide apple$ git checkout --track origin/develop
Branch 'develop' set up to track remote branch 'develop' from 'origin'.
Switched to a new branch 'develop'
apples-MacBook-Air:mygitguide apple$ git branch -vvt
* develop 17508c9 [origin/develop] commit 11
  master  8bcde89 [origin/master] commit 5

apples-MacBook-Air:mygitguide apple$ git checkout master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.
apples-MacBook-Air:mygitguide apple$ git branch -D develop
Deleted branch develop (was 17508c9).
apples-MacBook-Air:mygitguide apple$ git push --delete origin develop
To https://github.com/sankamuk/mygitguide.git
 - [deleted]         develop
```

### 11. Fork

- Adding original repository as remote upstream

```
apples-MacBook-Air:fork apple$ git clone https://github.com/sanmuk/mygitguide.git
Cloning into 'mygitguide'...
remote: Enumerating objects: 16, done.
remote: Counting objects: 100% (16/16), done.
remote: Compressing objects: 100% (10/10), done.
remote: Total 16 (delta 1), reused 13 (delta 1), pack-reused 0
Unpacking objects: 100% (16/16), done.

apples-MacBook-Air:fork apple$ cd mygitguide/
apples-MacBook-Air:mygitguide apple$ git remote -v show
origin	https://github.com/sanmuk/mygitguide.git (fetch)
origin	https://github.com/sanmuk/mygitguide.git (push)

apples-MacBook-Air:mygitguide apple$ git remote add upstream https://github.com/sankamuk/mygitguide.git
apples-MacBook-Air:mygitguide apple$ git remote -v show
origin	https://github.com/sanmuk/mygitguide.git (fetch)
origin	https://github.com/sanmuk/mygitguide.git (push)
upstream	https://github.com/sankamuk/mygitguide.git (fetch)
upstream	https://github.com/sankamuk/mygitguide.git (push)
```

- Synchronising upstream repository with local master.

```
apples-MacBook-Air:mygitguide apple$ git fetch upstream master
From https://github.com/sankamuk/mygitguide
 * branch            master     -> FETCH_HEAD
 * [new branch]      master     -> upstream/master
apples-MacBook-Air:mygitguide apple$ git merge upstream/master
Already up to date.
apples-MacBook-Air:mygitguide apple$
```

### 12. Pull or Merge Request

```
apples-MacBook-Air:mygitguide apple$ git remote -v show
origin	https://github.com/sanmuk/mygitguide.git (fetch)
origin	https://github.com/sanmuk/mygitguide.git (push)
upstream	https://github.com/sankamuk/mygitguide.git (fetch)
upstream	https://github.com/sankamuk/mygitguide.git (push)

apples-MacBook-Air:mygitguide apple$ git checkout -b fix-issue
Switched to a new branch 'fix-issue'
apples-MacBook-Air:mygitguide apple$ echo "Line15" >> sample.txt 
apples-MacBook-Air:mygitguide apple$ git commit -a -m "commit for pull"
[fix-issue 1126cc8] commit for pull
 1 file changed, 1 insertion(+)

apples-MacBook-Air:mygitguide apple$ git push origin fix-issue
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 4 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 297 bytes | 74.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/sanmuk/mygitguide.git 
```

Now from your forked repository(GitHub) create the the pull request to the upstream repository.



