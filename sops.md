# Secret OPerationS

## install
```sh
apt install golang
echo 'GOPATH=~/go' >> ~/.bashrc
source ~/.bashrc
mkdir $GOPATH

go get -u go.mozilla.org/sops/cmd/sops
```
```sh
pip install sops
```
## [visual code plugin](https://marketplace.visualstudio.com/items?itemName=signageos.signageos-vscode-sops)
```
ext install signageos.signageos-vscode-sops
```

## encrypt decrypt
```sh
sops -e -i myfile.json
sops -d myfile.json
```

```sh
sops -e myfile.json > myfile.json.enc
sops -d --input-type json myfile.json.enc
```

```sh
sops --encrypt --in-place --encrypted-regex 'password|pin' --pgp `gpg --fingerprint "vitalii@localhost.local" | grep pub -A 1 | grep -v pub | sed s/\ //g` myfile.yaml
sops --decrypt myfile.yaml
```

