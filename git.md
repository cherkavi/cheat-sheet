### debug flag, verbose output of commands, output debug
```
export GIT_TRACE=1
export GIT_TRACE=1
export GIT_CURL_VERBOSE=1
```

### clean 
```
git clean --dry-run
git clean -f -q
```

### restore
```
git reset --hard
```

### restore local branch like remote one
```
git reset --hard origin/master
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
git branch -d -f release-6.9.0
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

### check last commits for specific branch, last commits in branch
```
git log -5 develop
```
### check files only for last commits
```
git log -5 develop --name-only
```
### pretty log with tree
```
git log --all --graph --decorate --oneline --simplify-by-decoration
```

### show no-merged branches
```
git branch --no-merged
```

### checkout branch locally and track it
```
git checkout -t remotes/origin/release
```

### copy file from another branch
```
git checkout experiment -- deployment/connection_pool.py                                 
```

### set username, global settings
```
git config --global user.name "vitalii cherkashyn"
git config --global user.email vitalii.cherkashyn@wirecard.de
git config --global --list
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

### difference between two branches, list of commits
```
git rev-list master..search-client-solr
```

### git fetch
```
git fetch --all --prune
```

### find by comment
```
git log --all --grep "BCM-642"
```

### find by diff source, find through all text changes in repo
```
git grep '^test$'
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

### show changes by commit, commit changes
```
git diff {hash}~ {hash}
```

### git cherry pick without commit, just copy changes from another branch
```
git cherry-pick -n {commit-hash}
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

### show remote url
```
git remote -v
```

### change remote url
```
git remote set-url origin git@cc-github.my-network.net:adp/data-management.git
```


### clone operation under the hood
if during the access ( clone, pull ) issue appear:
```
fatal: unable to access 'http://localhost:3000/vitalii/sensor-yaml.git/': The requested URL returned error: 407
```
or
```
fatal: unable to access 'http://localhost:3000/vitalii/sensor-yaml.git/': The requested URL returned error: 503
```
use next command to 'simulate' cloning
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

### git lfs
```
echo 'deb http://http.debian.net/debian wheezy-backports main' > /etc/apt/sources.list.d/wheezy-backports-main.list
curl -s https://packagecloud.io/install/repositories/github/git-lfs/script.deb.sh | sudo bash
sudo apt-get install git-lfs
git lfs install
git lfs pull
```
if you are using SSH access to git, you should specify http credentials ( lfs is using http access ), to avoid possible errors: "Service Unavailable...", "Smudge error...", "Error downloading object"
```bash
git config --global credential.helper store
```
file .gitconfig will have next section
```
[credential]
        helper = store
```
file ~/.git-credentials ( default from previous command ) should contains your http(s) credentials
```file:~/.git-credentials
https://username:userpass@aa-github.mygroup.net
https://username:userpass@aa-artifactory.mygroup.ne
```

#### git lfs proxy
be aware about upper case for environment variables 
```
NO_PROXY=localhost,127.0.0.1,.localdomain,.advantage.org
HTTP_PROXY=muc.proxy
HTTPS_PROXY=muc.proxy
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

### download latest release from github, release download
```
curl -s https://api.github.com/repos/bugy/script-server/releases/latest | grep browser_download_url | cut -d '"' -f 4
```

### download last version of file from github
```
wget https://raw.githubusercontent.com/cherkavi/cheat-sheet/master/git.md
```

### linux command line changes
```
#git settings parse_git_branch() {
parse_git_branch() {
     git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/ (\1)/'
}
export PS1="\[\033[32m\]\W\[\033[33m\]\$(parse_git_branch)\[\033[00m\] $ "​
```

## hooks
### check commit message
```
mv .git/hooks/commit-msg.sample .git/hooks/commit-msg
```
```
result=`cat $1 | grep "^check-commit"`

if [ "$result" != "" ]; then
	exit 0
else 
	echo "message should start from 'check-commit'"
	exit 1
fi
```
