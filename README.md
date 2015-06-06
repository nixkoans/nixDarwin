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
