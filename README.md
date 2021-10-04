# ipfs-lfs-test
utilising https://github.com/sameer/git-lfs-ipfs we will test ipfs storage to
replace the limited github lfs storage

## IPFS
- install ipfs and start the daemon [see doco](https://docs.ipfs.io/)
- port forward 4001/tcp [see doco](https://docs.ipfs.io/how-to/nat-configuration/)

## Cloning
this requires your ``` ~/.gitconfig ``` to be updated to include this lfs
solution as an option before you will be able to pull the data, see the section
Configure (user) and Configure (repo) as you'll need to check the repo after 
initial cloning to ensure ipfs support is part of the lfs environment

## Build
```
apt-get install cargo
git clone https://github.com/sameer/git-lfs-ipfs
cd git-lfs-ipfs/git-lfs-ipfs-cli
cargo build --release
```

## Install
git-lfs [see doco](https://git-lfs.github.com/)
```
git lfs install
```
^required for unsupported ipfs-cli library function filter-process

git-lfs-ipfs-cli
```
cp ../target/release/git-lfs-ipfs-cli /usr/local/bin/

```

## Configure (user)
```
git config --global --add lfs.standalonetransferagent ipfs
git config --global --add lfs.customtransfer.ipfs.path git-lfs-ipfs-cli
git config --global --add lfs.customtransfer.ipfs.args transfer
git config --global --add lfs.customtransfer.ipfs.concurrent true
git config --global --add lfs.customtransfer.ipfs.direction both
git config --global --add lfs.extension.ipfs.clean "git-lfs-ipfs-cli clean %f"
git config --global --add lfs.extension.ipfs.smudge "git-lfs-ipfs-cli smudge %f"
git config --global --add lfs.extension.ipfs.priority 0

```

if you haven't already configured a default filter for lfs (maybe through git 
lfs) add one manually or your ipfs endpoints will not resolve

```
git config --global --add filter.lfs.clean "git-lfs clean -- %f"
git config --global --add filter.lfs.smudge "git-lfs smudge -- %f"
git config --global --add filter.lfs.process "git-lfs filter-process"
git config --global --add filter.lfs.required true

```

alternatively if you exclusively want to use ipfs for lfs, you will now no
longer need to configure each repo after cloning to use ipfs
```
git config --global --add filter.lfs.clean "git-lfs-ipfs-cli clean %f"
git config --global --add filter.lfs.smudge "git-lfs-ipfs-cli smudge %f"
```

## Configure (repo)
check you current lfs env 
```
cd <gitrepo>
git lfs env

```

tell you repo to use ipfs for lfs

```
cd <gitrepo>
git config filter.lfs.clean "git-lfs-ipfs-cli clean %f"
git config filter.lfs.smudge "git-lfs-ipfs-cli smudge %f"

```

## Testing
successfully cloning this repo will test your setup

## Usage
```
$ cd <git-repo>
$ git lfs track *.pdf
$ git add .gitattributes

```

## References
* https://github.com/git-lfs/git-lfs/wiki/Tutorial
* https://github.com/git-lfs/git-lfs/blob/main/docs/custom-transfers.md
* https://stackoverflow.com/questions/17888604/git-with-large-files
* https://docs.ipfs.io/


## Other
* https://github.com/sinbad/lfs-folderstore

