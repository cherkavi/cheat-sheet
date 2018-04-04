### clean 
git clean --dry-run
git clean -f -q

### restore
git reset --hard

### checkout with tracking
git checkout -t origin/develop

### show removed remotely
git remote prone origin

### remove branches that exist locally only ( not remotely )
git gc --pune=now

### remove local branch only
git branch -d release-6.9.0

### check hash-code of the branch
git rev-parse "remotes/origin/release-6.0.0"

### check all branches for certain commit ( git contains, git commit )
git branch --all --contains 0ff27c79738a6ed718baae3e18c74ba87f16a314
git branch --all --merged 0ff27c79738a6ed718baae3e18c74ba87f16a314
git log -1 develop

### show no-merged branches
git branch --no-merged

### checkout branch locally and track it
git checkout -t remotes/origin/release

### avoid to enter login/password
git config --global credential.helper store

### revert all previous changes with "credential.helper"
git config --system --unset credential.helper
git config --global --unset credential.helper

### show all branches merged into specified
git branch --all --merged "release" --verbose
git branch --all --no-merged "release" --verbose
git branch -vv

### difference between two commits ( diff between branches )
git diff --name-status develop release-6.0.0
git cherry develop release-6.0.0

### difference between branches for file ( diff between branches )
git diff develop..master -- myfile.cs

### git fetch
git fetch --all --prune

### find by comment
git log --all --grep "BCM-642"

### find file into log
git log --all -- "**db-update.py"
git log --all -- db-scripts/src/main/python/db-diff/db-update.py

### files in commit
git diff-tree --no-commit-id --name-only -r 6dee1f44f56cdaa673bbfc3a76213dec48ecc983

### difference between current state and remote branch
git fetch --all
git diff HEAD..origin/develop

### show changes into file only
git show 143243a3754c51b16c24a2edcac4bcb32cf0a37d -- db-scripts/src/main/python/db-diff/db-update.py

### git revert commit
git revert <commit>

### git revert message for commit
git commit --amend -m "<new message>"

### git show author of the commit
git log --pretty=format:"%h - %an, %ar : %s" <commit SHA> -1

### git into different repository
git --git-dir C:\project\horus\.git  branch --all

### clone only files without history
git clone --depth 1 https://github.com/kubernetes/minikube
