# sample-private

An exmaple of how to manage a public version from a private repository

## Clone repo

```
cheshyre@brandywine:Downloads$ git clone https://github.com/cheshyre/sample-private
Cloning into 'sample-private'...
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (3/3), done.
```

## Add public remote

```
cheshyre@brandywine:sample-private$ git remote add release https://github.com/cheshyre/sample-public
```

## Create "public" branch to sync with public repo

This should branch as early as possible to keep unwanted history out of the branch.
Easiest is to just go back to the first commit and create the branch there.

```
cheshyre@brandywine:sample-private$ git branch public
```

## Add some commits on main

```
cheshyre@brandywine:sample-private$ echo "New line 1\n" >> README.md
cheshyre@brandywine:sample-private$ git commit -a -m "Private commit 1"
[main 1eb072b] Private commit 1
 1 file changed, 2 insertions(+), 1 deletion(-)
cheshyre@brandywine:sample-private$ echo "New line 2\n" >> README.md
cheshyre@brandywine:sample-private$ git commit -a -m "Private commit 2"
[main 29648f4] Private commit 2
 1 file changed, 2 insertions(+)
cheshyre@brandywine:sample-private$ cat README.md
# sample-privateNew line 1

New line 2

cheshyre@brandywine:sample-private$ echo "# sample-private\n\nAn exmaple of how
to manage a public version from a private repository\n" > README.md
cheshyre@brandywine:sample-private$ git commit -a -m "Private commit 3"
[main 4b5b927] Private commit 3
 1 file changed, 2 insertions(+), 2 deletions(-)
cheshyre@brandywine:sample-private$ git log
commit 4b5b92784469ff30f97a583d4413bda93e0ec79a (HEAD -> main)
Author: Matthias Heinz <heinz.matthias.3@gmail.com>
Date:   Wed Jan 11 16:49:41 2023 +0100

    Private commit 3

commit 29648f44c5de8490b66396368753ee4b1b4ca976
Author: Matthias Heinz <heinz.matthias.3@gmail.com>
Date:   Wed Jan 11 16:48:35 2023 +0100

    Private commit 2

commit 1eb072b430a0d35972dd4f7b070a8817c2575c4c
Author: Matthias Heinz <heinz.matthias.3@gmail.com>
Date:   Wed Jan 11 16:48:11 2023 +0100

    Private commit 1

commit 94b1cad4eceb30f5f498399d9ca7099f56370c69 (origin/main, origin/HEAD, public)
Author: Matthias <cheshyre@users.noreply.github.com>
Date:   Wed Jan 11 16:42:12 2023 +0100

    Initial commit
```

## Switch to public and bring in changes from main via "squash"

```
cheshyre@brandywine:sample-private$ git checkout public
Switched to branch 'public'
cheshyre@brandywine:sample-private$ git merge --squash main
Updating 94b1cad..4b5b927
Fast-forward
Squash commit -- not updating HEAD
 README.md | 5 ++++-
 1 file changed, 4 insertions(+), 1 deletion(-)
 ```
 
 At this point, the changes are "staged", but not committed. We can commit them easily with a single commit message we desire.
 
 ```
 cheshyre@brandywine:sample-private$ git status
On branch public
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	modified:   README.md

cheshyre@brandywine:sample-private$ cat README.md
# sample-private

An exmaple of how to manage a public version from a private repository

cheshyre@brandywine:sample-private$ git commit -m "Update to version 1.0"
[public 80494ea] Update to version 1.0
 1 file changed, 4 insertions(+), 1 deletion(-)
cheshyre@brandywine:sample-private$ git log
commit 80494eacee2e9865fb56c541dd8034159aa49a68 (HEAD -> public)
Author: Matthias Heinz <heinz.matthias.3@gmail.com>
Date:   Wed Jan 11 16:55:02 2023 +0100

    Update to version 1.0

commit 94b1cad4eceb30f5f498399d9ca7099f56370c69 (origin/main, origin/HEAD)
Author: Matthias <cheshyre@users.noreply.github.com>
Date:   Wed Jan 11 16:42:12 2023 +0100

    Initial commit
```

## Push public branch to public repository

The `--force` here is only necessary if the public repository already has a commit history that you want to remove.
Do this only once (the first time) if at all. Afterwards you should be able to safely push normally.
```
cheshyre@brandywine:sample-private$ git push --force -u release public:main
Enumerating objects: 6, done.
Counting objects: 100% (6/6), done.
Delta compression using up to 8 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (6/6), 917 bytes | 917.00 KiB/s, done.
Total 6 (delta 0), reused 3 (delta 0), pack-reused 0
To https://github.com/cheshyre/sample-public
 + 8f4db9b...80494ea public -> main (forced update)
branch 'public' set up to track 'release/main'.
```

## Add more commits to main

```
cheshyre@brandywine:sample-private$ git checkout main
Switched to branch 'main'
Your branch is ahead of 'origin/main' by 3 commits.
  (use "git push" to publish your local commits)
cheshyre@brandywine:sample-private$ touch new_file.txt
cheshyre@brandywine:sample-private$ git commit -a -m "Private commit 4"
On branch main
Your branch is ahead of 'origin/main' by 3 commits.
  (use "git push" to publish your local commits)

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	new_file.txt

nothing added to commit but untracked files present (use "git add" to track)
cheshyre@brandywine:sample-private$ git status
On branch main
Your branch is ahead of 'origin/main' by 3 commits.
  (use "git push" to publish your local commits)

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	new_file.txt

nothing added to commit but untracked files present (use "git add" to track)
cheshyre@brandywine:sample-private$ git add new_file.txt
cheshyre@brandywine:sample-private$ git commit -m "Private commit 4"
[main 7560e51] Private commit 4
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 new_file.txt
cheshyre@brandywine:sample-private$ touch new_file2.txt
cheshyre@brandywine:sample-private$ git commit -m "Private commit 5"
On branch main
Your branch is ahead of 'origin/main' by 4 commits.
  (use "git push" to publish your local commits)

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	new_file2.txt

nothing added to commit but untracked files present (use "git add" to track)
cheshyre@brandywine:sample-private$ git add new_file2.txt
cheshyre@brandywine:sample-private$ git commit -m "Private commit 5"
[main 055e19c] Private commit 5
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 new_file2.txt
```

## Bring new changes in main to public and push them to public repo

```
cheshyre@brandywine:sample-private$ git checkout public
Switched to branch 'public'
Your branch is up to date with 'release/main'.
cheshyre@brandywine:sample-private$ git merge --squash main
Squash commit -- not updating HEAD
Automatic merge went well; stopped before committing as requested
cheshyre@brandywine:sample-private$ git commit -m "Update to version 2.0"
[public 68ebc98] Update to version 2.0
 2 files changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 new_file.txt
 create mode 100644 new_file2.txt
cheshyre@brandywine:sample-private$ git push -u release public:main
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 8 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 302 bytes | 302.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/cheshyre/sample-public
   80494ea..68ebc98  public -> main
branch 'public' set up to track 'release/main'.
```
