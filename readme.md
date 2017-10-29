# Git learning notes

## Dscription
This is just for git learning and exercising :).

I've learned a lot about git. The following is some commands that I used.

## Command
### intial
- `git init`
- `git config --global user.name`
- `git config --global user.email`

### add/commit/change/undo
- `git add file1.txt`
- `git commit -m "your commit"`
- `git diff xxx.txt`
- `git status`
- `git reset --hard commit_id`
- `git log`
- `git reflog`

### directory
- Directory: lgit\
- Repository: .git
- `git checkout -- file`(discard change that not be added)
- `git reset HEAD`(file that already be added)
- `git rm file`+`git commit -m "some notes"`(delete some file)

### remote repository
- Under the `User/.ssh/` there are 2 files: id_rsa(Personal Key) id_rsa.pub(Public Key). 

DO NOT TELL/SHARE ANYONE YOUR PERSONAL KEY！！！（sometimes you need type `chmod 600` if shell shows that "your key is open"）

### push
- `git remote add origin git@server-name:path/repo-name.git`(somehow it didn't work with `origin` so I change it to `origin1`. so were the following cases)
- `git push -u origin master`
- `git push origin master

### clone
- `git clone git@github.com:someuser/somework.git`(Note that the directory that file you cloned is related to your shell directory when running the command)