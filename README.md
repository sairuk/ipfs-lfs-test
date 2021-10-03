# ipfs-lfs-test
utilising https://github.com/sameer/git-lfs-ipfs we will test ipfs storage to
replace the limited github lfs storage

## Install ipfs
install ipfs and start the daemon see doco

## Cloning this repo
this requires your ``` ~/.gitconfig ``` to be updated to include this lfs
solution as an option before you will be able to pull the data, see the section
Configure (user)

## Build

```
apt-get install cargo
git clone https://github.com/sameer/git-lfs-ipfs
cd git-lfs-ipfs/git-lfs-ipfs-cli
cargo build --release
```
## Install
```
cp ../target/release/git-lfs-ipfs-cli /usr/local/bin/
git lfs install

```

## Configure (user)
```
git config --add lfs.standalonetransferagent ipfs
git config --global --add lfs.customtransfer.ipfs.path git-lfs-ipfs-cli
git config --global --add lfs.customtransfer.ipfs.args transfer
git config --global --add lfs.customtransfer.ipfs.concurrent true
git config --global --add lfs.customtransfer.ipfs.direction both
git config --global --add lfs.extension.ipfs.clean "git-lfs-ipfs-cli clean %f"
git config --global --add lfs.extension.ipfs.smudge "git-lfs-ipfs-cli smudge %f"
git config --global --add lfs.extension.ipfs.priority 0

```

## Configure (repo)
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

