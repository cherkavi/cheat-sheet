# Sync and Backup files cheat sheet
cli tools for making backups

## [rclone](https://rclone.org) - sync files with remove repo using 
* https://github.com/rclone/rclone
* https://rclone.org/docs/

### rclone installation
```sh
sudo apt install rclone 
rclone --version
```

### [rclone](https://rclone.org/#providers) config
```sh
vim $HOME/.config/rclone/rclone.conf
```

#### [rclone with google drive ](https://rclone.org/drive/)
```sh
rclone config
n
gdrive_vitalii
... 
```

#### [rclone with mega provider](https://rclone.org/mega/)
* Enter `n` to **create a new remote**.
* Enter a **name** for your remote (e.g., `mega_backup`).
* When prompted for the **Storage type**, enter the number corresponding to **Mega** (or type `mega`).
* Enter your **User name** (your MEGA email address).
* When prompted for the **Password**, enter `y` to type in your own password. You will enter and confirm your MEGA password. rclone will store this password **encrypted** in the configuration file.
* ...
* Finally, enter `y` to confirm and save the new remote.
```sh
rclone lsd mega_backup:/
```

### [rclone commands](https://rclone.org/commands/)
```sh
# https://rclone.org/commands/rclone_ls/
rclone ls gdrive_vitalii:/
rclone lsl --files-only --max-depth 1 gdrive_vitalii:/

# [rclone check size of repo with ncdu](https://rclone.org/commands/rclone_ncdu/)
rclone ncdu gdrive_vitalii:/

rclone copy gdrive_vitalii:/books/reading-repeat/interview/20210830_173607.jpg ~/Downloads/interview.jpg
rifle ~/Downloads/interview.jpg
```

#### rclone view files via browser
```sh
rclone serve http --addr 127.0.0.1:8000 gdrive_vitalii:/

x-www-browser http://127.0.0.1:8000/
```

### rclone rest api
```sh
# start http server: rclone serve http.... 
rclone serve http --addr 127.0.0.1:8000 gdrive_vitalii:/ --rc 

rclone rc --url http://127.0.0.1:5572 rc/noop
curl -X POST 'http://localhost:5572/rc/noop?potato=1&sausage=2'

rclone rc --url http://127.0.0.1:5572/ operations/list remote=gdrive_vitalii fs=/  opt='{"showHash": true}'

# start server to use for restic app
rclone serve restic
rclone serve restic --addr :8080 mega_backup:backup_folder
```

## [Restic](https://restic.readthedocs.io)
### installation
```sh
sudo apt install restic
```

### restic create repository
set default repo 
```sh
export RESTIC_REPOSITORY=rest:http://localhost:8080/
# export RESTIC_PASSWORD
```
```bash
## repository based on local folder
restic init -r /path/to/repo

## repository based on sftp
restic init -r sftp:user@host:/path/to/repo

## repository based on rclone 
RCLONE_REMOTE_NAME=gdrive_vitalii
PATH_IN_REMOTE=/my_folder
restic init --repo rclone:${RCLONE_REMOTE_NAME}:${PATH_IN_REMOTE}
```

### restic snapshot 
```sh
## create backup 
restic -r /path/to/repo backup /path/to/data

## list of all snapshots
restic -r /path/to/repo snapshots

## remove old snapshots
restic -r /path/to/repo forget --prune --keep-last 5

## restore snapshot to a target directory
restic -r /path/to/repo restore latest --target /path/to/restore

## check remote repo
restic -r /path/to/repo check
```

## [BorgBackup Borg](https://borgbackup.org)
* [tutorial](https://www.youtube.com/watch?v=lw4_jJsFUy8)
* https://github.com/borgbackup/borg
* https://borgbackup.readthedocs.io
```sh
# Create a new repository (repo)
borg init -e repokey /path/to/repo

# Create a new backup archive (snapshot) of a directory
borg create /path/to/repo::archive_name /path/to/backup

# List all archives in the repository
borg list /path/to/repo

# Remove old archives based on a retention policy
borg prune --keep-daily 7 --keep-weekly 4 /path/to/repo

# Extract a specific archive to a target directory
borg extract /path/to/repo::archive_name --to /path/to/restore

# Mount an archive as a FUSE filesystem for easy browsing
borg mount /path/to/repo::archive_name /mnt/borg

```
