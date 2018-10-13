### clean 
```
git clean --dry-run
git clean -f -q
```

### restore
```
git reset --hard
```

### remove last commit and put HEAD to previous one
```
git reset --hard HEAD~1
```

### checkout with tracking
```
git checkout -t origin/develop
```

### show removed remotely
```
git remote prone origin
```

### delete local branch, remove branch, remove local branch
```
git branch -D origin/release/2018.05.00.12-test
```

### delete remote branch, remove remote, remove remote branch
```
git push origin --delete release/2018.05.00.12-test
```

### remove branches, delete branches that exist locally only ( not remotely )
```
git gc --prune=now
git fetch --prune
```

### remove local branch only
```
git branch -d release-6.9.0
```

### check hash-code of the branch
```
git rev-parse "remotes/origin/release-6.0.0"
```

### check all branches for certain commit ( git contains, git commit )
```
git branch --all --contains 0ff27c79738a6ed718baae3e18c74ba87f16a314
git branch --all --contains {name-of-the-branch}
git branch --all --merged 0ff27c79738a6ed718baae3e18c74ba87f16a314
```

### check last commits for specific branch
```
git log -5 develop
```

### show no-merged branches
```
git branch --no-merged
```

### checkout branch locally and track it
```
git checkout -t remotes/origin/release
```

### avoid to enter login/password
```
git config --global credential.helper store
```

### revert all previous changes with "credential.helper"
```
git config --system --unset credential.helper
git config --global --unset credential.helper
```

### show all branches merged into specified
```
git branch --all --merged "release" --verbose
git branch --all --no-merged "release" --verbose
git branch -vv
```

### difference between two commits ( diff between branches )
```
git diff --name-status develop release-6.0.0
git cherry develop release-6.0.0
```

### difference between branches for file ( diff between branches )
```
git diff develop..master -- myfile.cs
```

### git fetch
```
git fetch --all --prune
```

### find by comment
```
git log --all --grep "BCM-642"
```

### current comment
```
git rev-parse HEAD
```

### find file into log
```
git log --all -- "**db-update.py"
git log --all -- db-scripts/src/main/python/db-diff/db-update.py
```

### files in commit
```
git diff-tree --no-commit-id --name-only -r 6dee1f44f56cdaa673bbfc3a76213dec48ecc983
```

### difference between current state and remote branch
```
git fetch --all
git diff HEAD..origin/develop
```

### show changes into file only
```
git show 143243a3754c51b16c24a2edcac4bcb32cf0a37d -- db-scripts/src/main/python/db-diff/db-update.py
```

### git revert commit
```
git revert <commit>
```

### git revert message for commit
```
git commit --amend -m "<new message>"
```

### git show author of the commit
```
git log --pretty=format:"%h - %an, %ar : %s" <commit SHA> -1
```

### git into different repository
```
git --git-dir C:\project\horus\.git  branch --all
```

### clone operation under the hood
```
git clone http://localhost:3000/vitalii/sensor-yaml.git
< equals >
wget http://localhost:3000/vitalii/sensor-yaml.git/info/refs?service=git-upload-pack
```

### clone only files without history, download code
```
git clone --depth 1 https://github.com/kubernetes/minikube
```

### download single file from repo
```
git archive --remote=ssh://https://github.com/cherkavi/cheat-sheet HEAD jenkins.md
```

### update remote branches, when you see not existing remote branches
```
git remote update origin --prune
```

### create tag 
```
git tag -a $newVersion -m 'deployment_jenkins_job' 
```

### push tags only 
```
git push --tags $remoteUrl
```

### configuration for proxy server, proxy configuration
#### set proxy, using proxy
```
git config --global http.proxy 139.7.95.74:8080
```
git config --global https.proxy 139.7.95.74:8080
#### check proxy, get proxy
git config --global --get http.proxy
#### remove proxy configuration, unset proxy
git config --global --unset http.proxy

### using additional command before 'fetch' 'push', custom fetch/push
git config core.sshCommand 'ssh -i private_key_file'

### remove auto replacing CRLF for LF on Windows OS
.gitattributes
```
*.sh -crlf
```
