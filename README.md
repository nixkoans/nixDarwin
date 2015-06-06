# Specifics of Nix package manager on Mac OS X 10.10 (Darwin)

## Run

```
curl https://nixos.org/nix/install | sh
source $HOME/.nix-profile/etc/profile.d/nix.sh
cd ~
git clone https://github.com/copumpkin/nixpkgs.git
cd ~/.nix-defexpr
rm -rf channels
ln -s ~/nixpkgs nixpkgs
```

Notice that I am using Daniel Peebles' fork of https://github.com/NixOS/nixpkgs.  This is because the current head of NixOS' nixpkgs is broken for darwin (Mac OS X). Daniel's `pure-darwin` fork contains all the necessary patches to build on Darwin correctly.  This is the current state as of 6th June 2015.  When a merge back into NixOS' nixpkgs happens, we can then switch to using that our machine.

## Check

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
