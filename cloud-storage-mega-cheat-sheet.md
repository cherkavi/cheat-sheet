## [mega installation ](https://mega.io/cmd)
```sh
# example for Ubuntu
wget https://mega.nz/linux/repo/xUbuntu_24.04/amd64/megacmd-xUbuntu_24.04_amd64.deb && sudo apt install "$PWD/megacmd-xUbuntu_24.04_amd64.deb"
```

## login 
```sh
mega-login $MEGA_USER $MEGA_PASS
```
check login
```sh
mega-whoami
mega-df
mega-du
```

## mega interactive mode
```sh
mega-cmd
```

## get file 
```sh
mega-ls
# mega-tree

# copy name of one of the file 
mega-get FILENAME
```