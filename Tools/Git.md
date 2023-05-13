# git token
* in github, choose setting
* choose developer setting
*  choose personal access tokens
* generate token to use
`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`
# git SSL connection
```
git config --global http.sslbackend schannel
```
# git single thread for ios
```unix
git config --global pack.threads "1"
```
# git init

-   git init
-   git init --bare

# git clone \[REPO_URI\]

# remote repository(exist), no local repository

steps for writing code in remote repository

1.  using **git clone "https://..."** to get remote repository code
2.  Write your code in local repository
3.  commit your code in local repository
4.  push your code into remote repository
5.  for non-empty remote repository, **git push origin master**
6.  for empty remote repository, **git push -u origin master**

# remote repository with exist local repository

steps for check existing code in remote repository

1.  **git remote add origin "https://..."** to connect local with remote
2.  push your code into remote repository
3.  for non-empty remote repository,
4.  **git pull origin master** (get the remote )
5.  **git push origin master**
6.  for empty remote repository, **git push -u original master**

# git add and reset to unstage files, and revert the file

-   git add \<files\> : to add file into stage
-   git reset HEAD \<files\> : to unstage file
-   git checkout HEAD \<files\> : revert the file

# git status

-   **git status --untracked-files=no** or **git status -uno**

# git revert file

```bash
git checkout HEAD -- my-file.txt

```

# git keywords in file

-   need to modify .git/info/attributes like follows:

```
# seee man gitattributes
*.cpp ident

```

then in *.cpp will have:

```cpp
/*
 * $Id: a3c0926847c6a014fabb59e85e70c8eeb25aff77 $
 */ 

```

# git branch

## see all branch

> git branch

## create branch

> git branch aa

## checkout brach

> git checkout aa

## git create and checkout branch

> git branch -b bb

## git delete branch

> git branch -d bb

## git merge branch

```bash
git checkout master
git merge aa

```

# git tag

## simple tag

```bash
git tag big_cats 51d54(commit number)

```

## annotated tag

```bash
git tag big_cats 51d54 -a -m "test for one"

```

## delete tag

```bash
git tad -d big_cats

```

# part of the depository

```bash
git clone --depth 1 --no-checkout "file://$(pwd)/server_repo" local_repo
cd local_repo
git checkout master -- mydir/project1

```
### emacs for git
```shell
M-x revert-buffer
```
# git diff
```shell
git log
commit fe6a
....
commit e802
....
git diff e802 fe6a MfgWidget.h
```
# git for remote replace
``` bash
git fetch --all
git branch backup-master
git reset --hard origin/main
git remote -v
git branch -r
git remote show origin
```