# Specifics of Nix package manager on Mac OS X 10.10 (Darwin)

Run:

```
curl https://nixos.org/nix/install | sh
source $HOME/.nix-profile/etc/profile.d/nix.sh
cd ~
git clone https://github.com/copumpkin/nixpkgs.git
cd ~/.nix-defexpr
rm -rf channels
ln -s ~/nixpkgs nixpkgs
```

Check:

```
nix-env -q
```

We should see these listed:

```
cacert-20140715
nix-1.8
```

Also, our `$NIX_PATH` should reflect that the `nixpkgs` path is available.

```
$ echo $NIX_PATH
nixpkgs=/Users/calvin/.nix-defexpr/channels/nixpkgs
```

## Use binary caches?

```
sudo mkdir /etc/nix
sudo vim /etc/nix/nix.conf
```

Add in the following lines in `nix.conf`:

```
binary-caches = http://zalora-public-nix-cache.s3-website-ap-southeast-1.amazonaws.com/ http://cache.nixos.org/ http://hydra.nixos.org/
```
