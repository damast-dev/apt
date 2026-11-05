# damast-dev/apt

Signed apt repository for [mast](https://github.com/damast-dev/mast), the command line for [Damast](https://damast.dev), served by GitHub Pages at **https://damast-dev.github.io/apt**.

## Install (Ubuntu, Debian; amd64 and arm64)

```sh
sudo install -d -m 0755 /etc/apt/keyrings
curl -fsSL https://damast-dev.github.io/apt/mast-archive-keyring.gpg | sudo tee /etc/apt/keyrings/mast-archive-keyring.gpg >/dev/null
echo "deb [signed-by=/etc/apt/keyrings/mast-archive-keyring.gpg] https://damast-dev.github.io/apt stable main" | sudo tee /etc/apt/sources.list.d/mast.list
sudo apt update && sudo apt install mast
```

The key is trusted for this repository only (`signed-by=`), not for your whole system. mast is a static binary, so one `stable` suite serves every Ubuntu and Debian release. It installs `/usr/bin/mast` and does not conflict with `fj`.

Every version stays in the index: `apt-cache policy mast` lists them, `sudo apt install mast=0.7.0-mast.1` pins one.

Remove:

```sh
sudo apt remove mast
sudo rm /etc/apt/sources.list.d/mast.list /etc/apt/keyrings/mast-archive-keyring.gpg
```

## How it is updated

The repository lives on the `gh-pages` branch. Don't edit it by hand. On every final mast release, [`release.yml`](https://github.com/damast-dev/mast/blob/main/.github/workflows/release.yml) in damast-dev/mast adds the new `.deb`s with [`apt-repo.sh`](https://github.com/damast-dev/mast/blob/main/.github/scripts/apt-repo.sh) (`apt-ftparchive`), signs `Release` / `InRelease` with the mast archive key, installs `mast` from the result in an `ubuntu:24.04` container, and pushes. Release candidates are not published here.
