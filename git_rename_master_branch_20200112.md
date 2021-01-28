
rename git branch
===
tags: git, rename branch

>description: 
>  create a new branch to instaed of master branch. 
>  treat as rename master branch


# ===============================
# follow below step to change the git branch name
```
step1: unprotect master
  gitlab web top-right of setting -> protected branched -> unprotect the master branch

step2: clone
  git clone https://projectlink
  cd projectname

step3: local branch rename from master to "default"
  git branch -a
  git branch -m master default
  git push origin default

step4: protect the main/default branch
  gitlab web top-right of setting -> protected branched -> protect the "default" branch
  gitlab web top-right of setting -> edit project -> choose the "default" branch for Default Branch setting

step5: [optional] set the remotes/origin/HEAD
  git remote set-head origin -a
  git branch -a

step6: [optional] delete master branch
  git push origin :master
```
# ===============================
# below is experiment with result.

gitlab web top-right of setting -> protected branched -> unprotect the master branch
```
#=exec=$  git branch -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/lts12p1_svn201214
  remotes/origin/master
  remotes/origin/release_LTS12p1_svn201214_mds1211
#=exec=$  git branch -m master default
#=exec=$  git branch -a
* default
  remotes/origin/HEAD -> origin/master
  remotes/origin/lts12p1_svn201214
  remotes/origin/master
  remotes/origin/release_LTS12p1_svn201214_mds1211
```
```
#=exec=$  git push origin default
Username for 'https://192.168.1.111': account
Password for 'https://account@192.168.1.111':
Total 0 (delta 0), reused 0 (delta 0)
remote:
remote: Create merge request for default:
remote:   https://192.168.1.111/account/ami_source_code/merge_requests/new?merge_request%5Bsource_branch%5D=default
remote:
To https://192.168.1.111/account/ami_source_code.git
 * [new branch]      default -> default
```
```
#=exec=$  git branch -a
* default
  remotes/origin/HEAD -> origin/master
  remotes/origin/default
  remotes/origin/lts12p1_svn201214
  remotes/origin/master
  remotes/origin/release_LTS12p1_svn201214_mds1211
```

gitlab web top-right of setting -> protected branched -> protect the "default" branch
gitlab web top-right of setting -> project -> choose the "default" branch for Default Branch setting
```
#=exec=$  git branch -a
* default
  remotes/origin/HEAD -> origin/master
  remotes/origin/default
  remotes/origin/lts12p1_svn201214
  remotes/origin/master
  remotes/origin/release_LTS12p1_svn201214_mds1211
```
```
#=exec=$  git remote set-head origin -a
Username for 'https://192.168.1.111': account
Password for 'https://account@192.168.1.111':
origin/HEAD set to default
#=exec=$  git branch -a
* default
  remotes/origin/HEAD -> origin/default
  remotes/origin/default
  remotes/origin/lts12p1_svn201214
  remotes/origin/master
  remotes/origin/release_LTS12p1_svn201214_mds1211
```
```
#=exec=$  git push origin :master
Username for 'https://192.168.1.111': account
Password for 'https://account@192.168.1.111':
To https://192.168.1.111/account/ami_source_code.git
 - [deleted]         master
```
```
#=exec=$  git branch -a
* default
  remotes/origin/HEAD -> origin/default
  remotes/origin/default
  remotes/origin/lts12p1_svn201214
  remotes/origin/release_LTS12p1_svn201214_mds1211
```




